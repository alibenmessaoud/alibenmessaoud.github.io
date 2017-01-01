---
layout: post
title: The Missing Layer
tags: modeling
---

Over the years, we saw many practices when developing applications, especially when using patterns such as MVC. When using this pattern, the model and its role in an application was unclear and misunderstood. Also, we saw and we will see tutorials showing the model, full of ORM annotations, used in a shared way across multiple layers of a project.

This confusion comes in three forms when writing code:

- Controllers: handle the business logic
  - developers said that it is straightforward approach and easy to understand
- Services: create services to hold the business logic
  - this style works with the MVC constructs and the logic is spread accross the model and the service but ...
- ORM Entities: entities hold the business logic
  - dirtiest approach, where infrastructural concerns mixed with domain and causing testing problems

Picking one of these options is problematic. None of these options are meaningful, yet the trap that most developers fall in is choosing the the first form: controllers hold the logic.

Let's look at this controller:

```java
public class IssueController {

  ...

  public List<IssueEntity> getAllIssues = (request, response) -> {
    return issueDao.getAllIssues()
  };

  public IssueEntity postNewIssue = (request, response) -> {
    if (isLoggedIn(request, respons)) {
      String summary = getQuerySummary(request);
      String author = getSessionCurrentUsername(request); // :o
      String description = getQueryDescription(request);
      IssueEntity issueEntity = new IssueEntity(summary, author, description);
      issueDao.insertIssue(issueEntity);
      return issue;
    }
    return null;
  };

  ...
      
  private boolean isLoggedIn = (request, response) -> {
    return logInController.ensureUserIsLoggedIn(request, response);
  }

  private String getSessionCurrentUsername = (request) -> {
    ...   
  }
}
```

This code is my code, which I wrote in the past. Unfortunately, there are several drawbacks and we can use it here to exhibit the problem in MVC:

- Repeated permission check in every single method
- Call `LogInController` inside a controller
- Returns are based on `IssueEntity`. Any change in the entity will break the code of the consumer of this API.
- Returns `null` value if the request is not authenticated. Not clear if it is `401` or `404` error.
- Validation is hardcoded in the controller through utility methods.
- Variables are named expressively. 
- Can store an issue with null as an author.

We need to solve these problems elegantly.

You may ask how? The answer is: Extract the infrastructural concerns. 

You may ask again ask how you would design the business logic layer? The keyword here is the business logic layer, BLL.

To answer this question, let's consider MVC again in our case. 

C is the `IssueController`, and M is the `IssueEntity`. We omit V right now as we return only JSON.

To enhance this code we need to find an equilibrium between C and M of M-C-V (or MVC, also ordered). 

Let's call it the missing layer of M-x-C-V. 

That mystery layer x contains the heart of our software.

The missing layer in MVC is the domain model. The model that contains the business rules of our domain. And it's what we should spend most of our time, thinking about how to craft it effectively.

M-C-V becomes RM-C-V, where RM stands for Rich Model. 

And why do we change M to RM? What is wrong with M?

M is diagnosed with "`oo-anemia`". It lacks enough of behavior and is used as a sate holder in an OOP world.  

M is known as Anemic Domain Model.

So What is an Anemic Domain Model?

A class with state exposed by getters and setters is an Anemic Model. An anemic model is often used as DTO object - a class with private fields and setters and getters.

Compared to the anemic model, a rich domain model does not provide the ability to modify fields externally. There are no setter methods. Variables have private access; they are set in a private constructor. Custom methods need to be created and used to update an object's state based on a set of rules. 

Moreover, factory pattern can be used to create new objects. Additionally, we need some getters to extract some information.

Now, you ask what should look like a controller in this case? 

The controller need to become stateless and provide its service by using the Rich Model methods. And it will be more useful when the workflow doesn't fit the current model. Except that, it must do what is asked by the model methods. 

The most important thing about this approach is that the rich domain object provides business methods, exposing the model's behavior and not just its state (as for standard models with getters/setters).