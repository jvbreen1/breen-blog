---
title: "Storyboards and Swift: A Happy Couple?"
date: 2016-03-14T00:00:00.000Z
description: >-
  I’m going to spend some time working with Swift and Storyboards in iOS.
---

I’m going to spend some time working with Swift and Storyboards in iOS.

My goal is to see if both technologies are mature enough and flexible enough to build production-quality iOS applications.

I have developed using Storyboards in the past, and I will admit up front that I am skeptical. Why am I skeptical?

## Maintainability

My experience with storyboards is that they are great in concept, but they quickly become unmaintainable and grow into fragile groups of tightly coupled components.

## Collaboration (Especially Git)

It is very difficult to understand Git diffs and commits involving Storyboard changes. This presents a problem when multiple developers are working on the same Storyboard, and it also makes it difficult to code review Storyboard changes.

## Drag and Drop

I’m generally not a fan of graphical, drag-and-drop UI editors. They seem to work well for simple cases, but I find myself falling back to editing the code when i want to do anything more complex than add a label or a button below an existing component.

A good example for me is with Android development. Android Studio provides a graphical way to edit UIs, but the XML it generates is also very easy to work with and modify. I wind up editing the Layout XML directly much more often than I am in the “Design” tab. As far as I know, there is no easy way to modify the Storyboard XML easily and programmatically.

# My Mission

At work, I spend a significant portion of my time developing in Objective-C. We create Views and View Controllers almost entirely programmatically, and it works quite well. We can adhere much more to traditional MVC patterns, and our code is modular, organized, and maintainable.

When I was learning iOS development, I used Storyboards and Objective-C. I was learning “on the job,” so I may not have been following best practices. Up front, I know I was probably committing Storyboard heresy by keeping almost the entire app in a single Storyboard. Even a cursory glance at Reddit and a quick search of my soul tells me this is bad. There are probably several more examples of my old bad habits, but I don’t want to embarrass myself any further and have my developer card revoked.

My goal in this adventure is to develop an app in Swift, using Storyboards, and to do it “the right way.” I plan on reading up on best practices when using Swift and Storyboards. Presumably, there isn’t a first-party way to solve problems like unintelligible Git diffs, but I will look for ways to mitigate these issues. I want to see if my opinions on Storyboards in particular are off-base (due to user error), or if these opinions are rooted in problems that are more systemic to the technology itself.

I Googled “Do you use Storyboard,” and I found [this Reddit thread](https://www.reddit.com/r/iOSProgramming/comments/1c5dno/do_you_use_storyboards/) that seems to have people arguing both sides of the point. Some use it and love it; some find it restricting and not useful for moderately complex apps. I’m currently in the latter camp, but I want to give it another shot and see if I can learn to like it.

If you want to follow along on GitHub, here’s the repository: https://github.com/jvbreen1/tip-tracker

This project mainly serves as a means for me to learn more about Swift and Storyboards. I don’t have any plans for releasing it to the App Store at this point in time.

# Conclusion

If you are interested in learning more about Swift and Storyboards for iOS development, please stay tuned! I intend on posting updates regularly as I make progress on the project.