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

Well, actually, the process is the same. I mean, it's



TODO: But do not focus too much on just improving the model