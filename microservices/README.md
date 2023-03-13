# 📦 Microservices

Earlier when developers used to work on applications they used to put all the logic in the same space - basically everything was **tightly coupled**. For example: you are building an e-commerce app which has a product page, payments feature, checkout page, etc and all of them are deployed as a single package. This is called the **monolithic** architecture.

![image](https://user-images.githubusercontent.com/55504616/224627386-75a737ad-2c04-419a-8355-79d61484895c.png)

Now the problem with this is that while scaling the application when changes are made to any one component like maybe the products page, everything else also gets affected by it even if there were no changes made in them. This is very inconvenient and can reduce performance.

The alternative was **microservices** architecture. The concept behind it is to break the application into smaller components so that each can be scaled individually. This means that the product page, checkout page, payments feature are now completely separated - meaning **loosely coupled**. If we want to make changes to any one compoennt or maybe we just want to scale any one of the components we can do that pretty easily.

![image](https://user-images.githubusercontent.com/55504616/224628166-716c947c-1df7-4c06-8044-9029fd44266b.png)

The rise of microservices made the use of container services like **docker** popular.

Now today microservices are implemented in 2 ways:

- Monorepo
- Polyrepo

In **monorepo** code for all the components are kept in the same repository but in separate folders. So still it's following the microservice architecture.

![image](https://user-images.githubusercontent.com/55504616/224629167-3e990757-9c36-4c7d-bf7b-4153f262caab.png)

In **polyrepo** we can create a separate repo for each component.

![image](https://user-images.githubusercontent.com/55504616/224629859-515a9a09-a4f3-4e17-96f9-d53a02217b5a.png)

