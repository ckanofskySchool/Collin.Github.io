---
title: Hardware Design
subtitle: Planning and Designing Hardware for the project
image: assets/img/portfolio/03-full.jpg
alt: 

caption:
  title: Hardware Design
  subtitle: Planning and Designing Hardware for the project
  thumbnail: assets/img/portfolio/03-thumbnail.jpg
---
Use this area to describe your project. Lorem ipsum dolor sit amet, consectetur adipisicing elit. Est blanditiis dolorem culpa incidunt minus dignissimos deserunt repellat aperiam quasi sunt officia expedita beatae cupiditate, maiores repudiandae, nostrum, reiciendis facere nemo!

{:.list-inline}
- Date: January 2017
- Client: Finish
- Category: Identity



## MakeraCAD thoughts

For my project, I used a pocket cut between the traces and the edge, and while that makes the end result very clean and have nothing but traces left, it is very time consuming to cut every single part except traces out. 

When I thought some more about this situation, I wondered if I could use a contour instead to mill just the area around the traces, and take way less time. However, this concept had an isssues, the 2D Contour Toolpath only creates one path around the traces which might not be enough and additionaly, only one mill bit could be selected, unlike the pocket where a larger and detail bit could both be selected.

One interesting solution to this I thought up was to have 2 contours, the first being a .8mm Corn bit contour which had a .1mm offset, meaning another countour cut with the .2mm detail bit ould take off just the edges achieving good space around each path while also having the nice detailed cut. I have yet to test this theory, but hope to do so.