# Junior-middle Data Science / Machine Learning developers from the company perspective

### [2021] My lecture for first and second year students of IT and related faculties
---
[Answers to pre-prepared student questions]

---


## - How should I start a career in ML/DS? What do I need to learn to be a useful junior developer

You should start by learning utilities and services used in product envinronment! You have to be familiar with such things as git, Jira, docker, AWS. Know well the programming language you are using, that could be Python, R, or Scala, also you need to know SQL. Some specific libraries for your favorite language, like numpy, scipy, and pandas in case if your favorite language is Python. Outsite of IT it's good to know basics of statstics and linear algebra.

I would not put too much focus on the math part or even libraries, because, let's be honest - all the experience of more than a half of students in this room is a [Kaggle](https://www.kaggle.com/) playground with a theoretical knowledge from a couple of DS/ML/AI cources. The main problem is that the environment is too polished by competition hosts. Basically, you're getting a well-formatted CSV file and your output is another CSV file, where you usually have a standalone python script in the middle.

In reality, you're spending 60%+ of your time on cleaning and formatting the data, and I am not talking about feature formatting, but this extraction from the raw data. Sometimes you do not even have the raw data and should create it first, and for that, you need to know such frameworks and libraries as Scrapy and lxml. Here also needed some kind of database for storing the data and docker-compose for combining your parser and database.

It is very hard to imagine an isolated DS developer in a large project who asks for a clean CSV file based on some joined tables made by devops or someone else with access, and then codes a single .py script with the lines:

```python
data = pd.read_csv(“data.csv”)
…
model = Classifier()
model.fit(X, y)
predictions = model.predict(X_test)
```

and after that drops this snippet in the backend slack channel with a note "my job here is done, good luck with MR / integration".

Such things could work only if that developer has a PhD and works in fintech in the role of a quantitative analyst. They usually have their own micro-team which is always ready for translating rough drafts in R or TeX into the project's main programming language, finding or creating required libraries, testing the code, and so on...

In general, DS/ML developer has to know everything what your avegare backend-developer know. REST principles, coding conventions, merge request process, testing, deploying, have an idea of how to build microservices, getting or requesting the needed data on your own, and so on.

---

## - What is the main difference in the workflow of a quantitative analyst?

The data is usually structured and clean, it's like in your favorite Kaggle competition. I mean, it's just ineffective from a company perspective to a ask $200.000+/year guy to spend his time on writing scrappers or performing OCR on a bunch of PDF files to collect some useful data. Here comes a Kaggle approach as well, such as finding the best weights by doing a full grid search, choosing the right models and their ensembles, deep feature engineering, regularizations, analysis of the statistical distributions, and so on. Because the goal is different - you usually need to improve some kind of Sharpe ratio from 0.21533 to 0.21600 and this is all that is needed from you. The main logic of the project, the method of creating features, and even their names are usually hidden from an analyst for security reasons. In this particular case, the isolation of this person from the rest of the team is entirely justified, it's better for everyone when nobody has to know what is going on with these hand-written drafts in the future workflow.

---
## - Alright, omitting quants, how do I improve my model in the production environment? Any examples of ML stacks in production?

Well, actually, the process is the same. Starting with data massaging, doing all sorts of basic transformations like normalizing the data and removing outliers... choosing metrics, algorithms, and features, and creating some kind of PoC model of 20 lines of code... and nothing stops you to start searching for the best weights and ensembles for the next couple of weeks which can add a tiny fraction of a percent to the chosen metric. And that may sound impactful when at the end of each sprint you have a "Model weights have been updated, microserviceABC:metrics@k +0.005%" in the presentation after smashing the "Start grid-search" button.

But is this impactful on the company level? Not really. Do not focus too much on just improving the model.

Let us assume that you need to update the old recommendation service for an e-shop. If you follow the popular tutorials and practices you need to get the data enough to create a "userXproduct->rating" matrix. The next steps usually contain some choosing "the best" distance metric which could be cosine, Euclidean, Pearson... After playing a couple of days with metrics you may suddenly realize that your matrix filled with the production data will cause "out of memory" on any existing instance in the world and you may want to add SVD/PCA and start to hyperoptimise all possible parameters of them for another couple of days or weeks...

And this will continue until someone from the upper management will notice a drop or stagnation in revenue or until someone from the manual testing team found out that recommended products are not really relevant. Or both of these discoveries will happen, resulting in an investigation of lacking correlation between recommendation metrics and sales.


* Checking that the microservice was used as it was designed by the DS team. And for that you need to know the main programming language of the core project, and also know how to use tools such as Postman to query the production API. Ideally, someone from the DS team should be in response to merging the service into the core platform (ML Ops).

* Testing on historical data may give incorrect results if you do not know the full history of the project. For example, in the process of testing you may assume that the old recommendation system always returned exactly 10 of the most relevant records because these records were logged on the backend-side, so the whole "improvement metrics" is based on a comparison of "old 10 vs new 10" records. In reality, these "old 10" records have been halved on the frontend, which took the responsibility for throwing out irrelevant records at the end of the recommendations list. So you need to interact with multiple team.

* The metric itself is incorrect, or you simply do not have enough data. It's good to know the UI/UX part and the full customer workflow, and how buyers/customers are interacting with the platform. Like, for example, you can create a Jira ticket with the request of adding a new button, that allows customers to hide irrelevant results, or ask the frontend team to add click tracking. Also, do not hesitate to ask the sales dept./upper executive about their view of the "metric of success". That can be a number of sales/month, a growing rate, a nubmer of new clients, or some completely unexpected ratio. It's quite useful to build a correlation between this "success metric" and metrics from the ML models/backtesting.

* Additional recommendation services are needed. For example, you may recommend products by considering their semantic similarity, and here comes the NLP. Or you can add a taxonomy of products such as UNSPSC, and build a classification on top for products with the unknown UNSPSC code (without playing way too much with weights and model ensembles this time, please). You can augment the data to the point when the product can be recommended as a "the client often bought the product X from the manufacturer Y, so let's also recommend the top [N_1] popular products of the manufacturer Y, where 30% of them will be in the same UNSPSC category of level [1-4] with the product X, and the rest 70% will be semantically similar to X. Also let's take into account all other products that were clicked by the customer, seasonality, discounts, the distance between a client and manufacturer/supplier, delivery availability, number of reviews and so on and so forth. For that, you have to know the industry you are working in.

Speakig of the tech. stack, it could be 

`DataLake` → `MLFlow`/`AWS_Sagemaker`(docker_image(`fastAPI`(model)→`REST`)) <-> core app. + `Elasticsearch` + BI tool (`Looker`/`Tableau`) + `Datadog` + `Grafana`

you can also add `Apache software` in case of high load, but I have not seen cases where such tools as `Spark` or `Hadoop` were really needed in small/medium projects, the bandwidth of the input data is often overestimated during the design phase. The next steps will be about adding CI/CD, versioning, and finalizing the full model release flow -- all of that will be on the MLOps. Also adding routes for A/B testing, and snapshots of the whole platform for the proper historical testing (not only the data from the database but also all other settings that could affect the recommendations) -- this one will be on the DevOps and the backend team

This will allow us to create 3 layers of analytics:
* Platform level (logs, microservices, and their metrics -- Datadog, Grafana, Sagemaker/MLFlow), 
* Developers level (team performance -- Jira, Lattice)
* Company level ("success metrics" -- BI tool for top managers and investors)


What I am trying to say is that following any of these points of the checklist will be more impactful than trying to finetune the single model for another sprint or two. So, when I was hiring DS/ML developers I was focused on the skills of a candidate which are not really related to DS/ML at the first glance. In a way, it is easier to teach ML to someone with the backend developing experience, than the other way around. Because you will have to, sooner or later.

Of course I do not ignore completely math skills, for example, it is good and kind of required to understand distributions, outliers, the geometric interpretation of classification and regression, as well as the ability to select appropriate metrics and algorithms. But I don't see much point in asking the questions like “How are trees built in gradient boosting?” or something like that.


---
## - What did you add to the test task? How do you rate the assignment solution?

Something practical, creating a cleaning module for the data of a given format. You can say a lot by looking at the way the candidate is cleaning the data, like can these methods be reusable or not. For example, the code may contain logic such as "at line 10 of the CSV file remove the last 5 symbols", which is a pretty bad approach. Or it can contain decent regexes with 3rd party libraries, that can be added straight to the real project.
The task itself is pretty abstract, and it was created with the logic that for the best results you need to augment the initial data, which may require adding a scraper, mapper, and classifier, but all of this is not required. 

Students usually tend to use their own code, which results in 1000+ lines of unreadable code in the module (which I usually can not run due to wrong import paths/different local environment/lib versions, in other words, the most common problem is a lack of a docker image).
It seems strange, but understandable due to lab requirements at the university where you have to strictly follow the given instructions with your very own solutions.

Personally, I do not really mind using 3rd party libraries, ready-to-use solutions from Github, and StackOverflow copy-pastes in test task solutions as long as they are solving the business goal.