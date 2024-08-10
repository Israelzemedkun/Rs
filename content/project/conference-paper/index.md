---
title: 'Low-Cost Automated Basketball Score Tracking System'

# Authors
# If you created a profile for a user (e.g. the default `admin` user), write the username (folder name) here
# and it will be replaced with their full name and linked to their profile.
authors:
  - admin

# Author notes (optional)
author_notes:
  - 'Equal contribution'
  - 'Equal contribution'

date: '2022-11-01T00:00:00Z'
doi: ''

# Schedule page publish date (NOT publication's date).
# publishDate: '2017-01-01T00:00:00Z'

# Publication type.
# Accepts a single type but formatted as a YAML list (for Hugo requirements).
# Enter a publication type from the CSL standard.
#publication_types: ['paper-conference']

# Publication name and optional abbreviated publication name.
#publication: In *Hugo Blox Builder Conference*
#publication_short: In *ICW*

abstract: This project presents the development of a low-cost, automated basketball score recording system designed for recreational and training purposes. By leveraging an Arduino Uno microcontroller and an ultrasonic sensor, the system aims to accurately detect and record successful basketball shots. The project addresses the need for affordable and efficient data collection methods in sports analytics, focusing on creating a solution that is both cost-effective and simple to deploy. The system's performance was evaluated through various tests, achieving a 76% accuracy rate in shot detection, demonstrating its potential to enhance data collection and analysis in basketball training and recreational play.



# Summary. An optional shortened abstract.
summary: This project developed a cost-effective automated system for recording basketball scores using an Arduino Uno and an ultrasonic sensor. The system achieved a 76% accuracy rate in detecting successful shots, providing a practical solution for enhancing data collection in sports training and recreation.

tags:
  - Large Language Models

# Display this page in the Featured widget?
featured: true

# Custom links (uncomment lines below)
# links:
# - name: Custom Link
#   url: http://example.org

url_pdf: ''
url_code: '#'
url_dataset: ''
url_poster: ''
url_project: ''
url_slides: ''
url_source: ''
url_video: ''

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
#image:
#  caption: 'Image credit: [** **](https://)'
#  focal_point: ''
#  preview_only: false

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
projects:
  - example

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
slides: example
---

# Low-Cost Automated Basketball Score Tracking System

### Israel Gebre and Abel Kidane Habte

#### Monday, 9 May 2022

---

## 1. Introduction

Ever thought about making a basketball scoreboard that doesn’t cost an arm and a leg? Well, that’s what this project is all about! We set out to create a low-cost, automated basketball score tracking system that anyone could use for practice sessions or casual games. We broke it down into two main parts: first, figuring out when a ball actually goes through the hoop, and second, finding the cheapest way to collect and use that data.

Sports data is a goldmine, and while most of it focuses on player performance and game outcomes, we wanted to explore something different—something that could help you rack up more points in a game or understand how the sport is growing in your neighborhood. We figured that by building a simple, affordable system, we could gather data from all sorts of local games, making sports science more accessible to everyone.

Sure, the main goal was to build a low-cost, battery-powered basketball score tracker, but we also hoped this little project might contribute to the bigger picture of sports analytics. Using basic tech like this could open up new ways to understand how basketball is played in different places, giving us insights that more expensive, sophisticated systems might miss.

---

## 2. Problem Statement and Background

Basketball’s popularity is booming worldwide. It’s a game for all ages, whether you’re playing in a team or just shooting hoops at the park. But keeping track of scores, shots, and misses? That’s a hassle. It takes years of practice to perfect your shooting, and without good feedback, it’s hard to improve.

That’s where the idea of “smart balls” comes in—they’ve got sensors inside that can tell if you made a shot or not. But they’re expensive, not super durable, and if you want to practice with more than one ball, you’ll need to buy more smart balls. So, we thought, why not come up with something cheaper and easier to use?

---

## 3. Existing Automated Scoring Technologies

There are already some cool systems out there that use lasers, sensors, and even photoelectric cells to track basketball shots. But most of these are pricey and complicated. They often struggle with accuracy because they can’t always tell the difference between a basketball and, say, a player’s hand.

For example, one system uses sensors in the hoop’s net to detect vibrations when a ball goes through. Another uses a light beam that gets broken when a shot is made. Both of these methods have their downsides—like false readings from bouncing balls or interference from other objects.

---

## 4. System Design

We wanted to keep things simple and cheap, so here’s what we used for our automated basketball score tracker:

1. **Microcontroller Board:** We went with an Arduino Uno R3. It’s affordable, easy to program, and can handle all the basic inputs and outputs we need.

2. **Sensor:** We used an ultrasonic sensor to detect when a shot is made. It’s cheap, reliable, and gets the job done.

3. **LCD Display:** This shows the score as you play.

4. **Other Components:** We also used a few resistors, a 9-volt battery, some jumper wires, and a breadboard to put it all together.

### 4.1 Microcontroller Board

The Arduino Uno is the brains of our system. It’s a simple microcontroller that can read inputs like light, sound, or in our case, distance from an ultrasonic sensor, and then turn that into outputs—like displaying the score on an LCD screen. It’s cheap, widely available, and has a user-friendly programming environment.

### 4.2 Sensor

We mounted an ultrasonic sensor under the basketball hoop to detect when a ball goes through. The sensor sends out sound waves and measures how long it takes for them to bounce back. If something (like a basketball) interrupts the sound wave within a certain distance, the system registers a score.

### 4.3 Accounting for Shot Values and Fouls

To take things a step further without breaking the bank, we brainstormed ways to account for shot values (2 or 3 points) and fouls:

- **Pressure Mats:** These could be placed around the three-point line to detect whether a player’s foot is on or inside the line. They’re not too expensive and are easy to set up.

- **Contact Sensors:** Players could wear simple contact sensors to detect when fouls occur. These sensors could pick up on significant physical interactions and help the system decide when to flag a foul.

---

## 5. Observation

We built a prototype of our basketball score tracker and tested it out by shooting and dropping the ball through the net. We ran the system six times, taking over 60 shots each time, and found that it had an accuracy rate of about 76% in its current state. We used Microsoft Data Streamer to collect and analyze the data, which was easy to set up with our Arduino.

---

## 6. Conclusion

In this fun side project, we created a low-cost, automated basketball score tracking system. The system uses an Arduino Uno and an ultrasonic sensor to detect when a ball passes through the net. We also explored some cool, cost-effective ways to account for shot values and fouls, like pressure mats and contact sensors. While the system isn’t perfect yet, it’s a solid start and could be a great tool for anyone looking to improve their game or just keep track of scores without all the hassle.

---

## 7. Future Work

We’ve got some ideas for taking this project to the next level:

- **Improving Accuracy:** We could tweak the sensor placement and algorithms to boost the system’s accuracy.

- **More Advanced Data Collection:** By adding more sensors or refining the existing ones, we could collect even more detailed data—like shot speed or angle.

- **Mobile App Integration:** How cool would it be to have a companion app that tracks your scores and gives you feedback in real-time?

This project is just the beginning. With some more work and a bit of creativity, we think it could be a game-changer for anyone who loves basketball but doesn’t want to spend a fortune on high-tech gear.

---

**References**
- Schumaker, R. P., Solieman, O. K., & Chen, H. (2011). Sports Data Mining. In *Sports Data Mining* (pp. 1–15). Springer.
- Klein, W. M. (2009). Real-Time Wireless Sensor Scoring.
- Best, J. L. (1989). Score-Sensitive Basketball Hoop.
- Huang, S. Y. (2004). Method and Device for Detecting Goal in Basketball Game Device.
- Thurman, R. T., Krysiak, K. L., & Proeber, D. J. (2014). Basketball Sensing Apparatus.
