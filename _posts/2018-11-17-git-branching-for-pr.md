---
layout: post
title: Git Branching for PR
tags: git cli
---

In the past, I struggled with the code review, and the merge of large pull requests as an author and also as a reviewer. In this article, we will see a simple technique to create branches and get the code review easier overtime. 

For complex requests that should be merged in the main branch (`master` or any other principal branch) as a single or two commits but they are too large to fit in one understandable pull request, consider a stacked pull request model:

The idea is to create a primary branch `feature/XXXX-YYYY` and then create a number of secondary branches to be merged in your primary branch. For example, if you do frontend and backend development at the same time, you can create `feature/XXXX-YYYY-api` for the backend and `feature/XXXX-YYYY-js` for the frontend. Then, get them individually code-reviewed against the `feature/XXXX-YYYY` branch. So, when everything is OK, you can merge, continue the work, and thus will help you to keep pull requests light and focused on a specific feature.

Also, you are not forced to create all the secondary branches at the same time. Indeed, you can create and merge branches into the primary branch as things progress. For example, you can create and work on the `feature/XXXX-YYYY-api` then get it reviewed. Once it is merged, you can create `feature/XXXX-YYYY-js` and start working on it and get it merged in the end. Or, if you like to work the frontend first then you can work on `feature/XXXX-YYYY-js` with mocked server data. 

There are many ways to work and create secondary branches in a sequential or parallel way. 

Finally, once all secondary branches are merged into `feature/XXXX-YYYY`, create a pull request for merging the latter into the main branch.

If you’ve read this far, I hope that this article has helped you realize that too.