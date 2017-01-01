---
layout: post
title: Presentational vs Container Components
tags: reactjs react patterns
---

In this article, we'll talk about the difference between Presentational and Container Components in React.

In React components are usually split into 2 big categories: presentational components and container components.

Each of those has its unique characteristics.

Presentational components are essentially concerned with receiving data and callbacks exclusively via props, and producing some markup to be outputted.

Also, they have no dependencies on the rest of the app. They don’t handle any state, excluding for state related to the presentation.

Container components are principally concerned with how things work like the “backend” operations.

They provide the data and behavior to presentational or other container components. They might manage the state of various sub-components. They might wrap many presentational components.

As a way to clarify the difference, we can assume presentational components are involved with the look, container components are concerned with making things work.

For example, this is a presentational component. It gets data from its props, and just focuses on showing an element:

```typescript
const PersonList = ({persons}) => (
  <ul>
    {persons.map(person => (
      <li>{person}</li>
    ))}
  </ul>
)
```

On the other hand, this is a container component. It manages and stores its own data, and uses the presentational component to display it.

```typescript
class Persons extends React.Component {
  const [persons, setPersons] = useState([]);

  componentDidMount() {
    PersonService.getAll().then(users =>
      persons.setPersons(persons))
    )
  }

  render() {
    return <PersonList persons={persons} />
  }
}
```

Following this approach helps in better separation of concerns. We can easily understand the app and the UI better by writing components this way.

Other names: Stateful and Stateless, Classes and Functions;