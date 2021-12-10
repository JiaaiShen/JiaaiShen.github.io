---
layout: post
title: Reflection Blog Post
---

In this blog post, I'll reflect on the experience of completing the project.

## Questions

- Overall, what did you achieve in your project?

We write this part as a group.

We were able to visualize a large cardiovascular disease data set, build machine learning models, use the most performant model to predict whether an individual is at risk of cardiovascular disease given his personal data, and show the result in a webapp.

- What are two aspects of your project that you are **especially proud** of? 

1. I am especially proud of the social significance of our project. It is very important to detect cardiovascular disease as early as possible. Cardiovascular disease is the leading cause of death globally. One person dies every 36 seconds in the United States from cardiovascular disease. However, most cardiovascular disease can be prevented by addressing risk factors such as obesity, smoking, use of alcohol, and phyisical inactivity, all of which are included in the features that our model and webapp use to predict whether an individual is at risk of cardiovascular disease or not. Therefore, our project can help the society and save lives.


2. I am also especially proud of our project's successful incorporation of a machine learning model into a webapp. This is the most challenging part of our project. We tried to pickle a TensorFlow model first, then tried to save the weights of a TensorFlow model, but all these attempts failed. As a result, we decided to use a multinomial logistic regression model instead. Although we were able to save the model, there still were issues with our webapp. For instance, our model required two-dimensional numerical rather than one-dimensional textual input, and a prediction of 0 (absence of cardiovascular disease) would stop our webapp from showing the prediction since 0 was equal to false. With Prof. Chodrow's help and our persistence in debugging, finally, we made it! Our webapp is now able to predict whether an user is at risk of cardiovascular disease or not with a multinomial logistic regression model and show the prediction result to the user.

- What are **two things** you would suggest doing to further improve your project? (You are not responsible for doing those things.) 

1. I would suggest deploying our webapp online by Heroku so that everyone can have easy and direct access to our webapp and everyone can have a chance to find out whether he is at risk of cardiovascular disease or not.


2. I would suggest trying other kinds of machine learning models and finding a larger cardiovascular disease data set to improve the accuracy of our model. Now, the accuracy of our most performant model on the unseen test data set is 73%. Although this accuracy is higher than the accuracy of the baseline model, our webapp is still about 30% likely to fail to make accurate predictions. It will be alarming if an individual is actually healthy but our webapp tells him to see his doctor. It will be dangerous if an individual is actually at high risk of cardiovascular disease but our webapp thinks he is healthy.

- How does what you achieved compare to what you set out to do in your proposal? (if you didn't complete everything in your proposal, that's fine!)

We write this part as a group.

1. We proposed to make predictions for multiple diesases but ended up focusing on cardiovascular disease.


2. We proposed to produce a risk index to show the level of risk of suffering from a certain disease but ended up producing a binary result indicating whether a user is at risk or not.


3. We proposed to provide several personalized infographics of a certain disease for a user but ended up giving textual information.

- What are **three things** you learned from the experience of completing your project? Data analysis techniques? Python packages? Git + GitHub? Etc? 

1. I learned to use Git and GitHub for group collaboration and version control. With Git and GitHub, I could share my work with my group members anywhere and anytime and track all the changes made to our files and notebooks easily.


2. I learned to use data visualization, such as creating a box-and-whisker plot and a kernel density estimate (KDE) plot, to find outliers in a data set. By removing the outliers, we can improve the accuracy of machine learning models applied to the data set.


3. I learned to use CSS to design a very attractive webapp. CSS allowed us to change the fonts and colors such as changing the background color of the input box from white to transparent, adjust the line spacing, and create an animated background.

- How will your experience completing this project will help you in your future studies or career? Please be as specific as possible. 

This project experience has provided me with precious opportunities to practice data analysis and machine learning skills such as using Pandas, Scikit-learn, and Seaborn and learn new Python packages like TensorFlow, all of which are important for my future studies and career since I am applying for data science master program and want to become a data scientist in the future. There surely will be bugs in my future data science work. With this project experience in mind, I will google the error messages, see whether other people have met similar problems before, and try their ways of debugging. This method will save me a lot of time. Moreover, there were several project checkpoint acitivities like presentations throughout this quarter. They functioned as reminders to me of working on the project. In the project proposal we submitted at the beginning, we created a tentative timeline as well. When working on my future projects, I will follow this practice and make plans for myself and my team to improve efficiency and avoid procrastination.
