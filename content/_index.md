---
# Leave the homepage title empty to use the site title
title: "Israel"
date: 2022-10-24
type: landing

design:
  # Default section spacing
  spacing: "3rem"

sections:
  - block: resume-biography-3
    content:
      # Choose a user profile to display (a folder name within `content/authors/`)
      username: admin
      text: ""
      # Show a call-to-action button under your biography? (optional)
      button:
        text: Download CV
        url: uploads/resume.pdf
    design:
      css_class: dark
      background:
        color: black
        image:
          # Add your image background to `assets/media/`.
          filename: stacked-peaks.JPG
          filters:
            brightness: 1.0
          size: cover
          position: center
          parallax: false
  - block: markdown
    content:
      title: '📚 My Research'
      subtitle: ''
      text: |-
              My research is focused on the critical intersection of environmental sustainability, data science, remote sensing, and machine learning. Hailing from Ethiopia, a nation renowned for its rich biodiversity yet confronting significant environmental challenges, I am deeply committed to advancing sustainable solutions that address these pressing issues.

              In my role as a Remote Sensing Research Assistant at Ashoka University, I developed sophisticated machine learning models for detecting vegetation health and air pollution using satellite data. This work involved refining analytical methodologies to enhance the precision and accuracy of these models, contributing to more effective environmental monitoring.

              During my externship with the National Geographic Society and The Nature Conservancy, I conducted comprehensive landscape and gap analyses to support freshwater conservation initiatives. These experiences have solidified my dedication to leveraging technology in the pursuit of environmental sustainability.

              I am passionate about exploring how cutting-edge technological tools can drive sustainable environmental practices and am continually seeking opportunities for collaboration. I invite like-minded professionals to connect with me to explore innovative solutions to the environmental challenges we face today.      
    design:
      columns: '1'
  - block: collection
    id: papers
    content:
      title: Current projects
      filters:
        folders:
          - publication
        featured_only: true
    design:
      view: article-grid
      columns: 2
  - block: collection
    content:
      title: Highlight 
      text: ""
      filters:
        folders:
          - publication
        exclude_featured: false
    design:
      view: citation
  - #block: collection
    #id: talks
    #content:
      #title: Recent & Upcoming Talks
      #filters:
        #folders:
        #  - event
    design:
      view: article-grid
      columns: 1
  - block: collection
    id: news
    content:
      title: Resurces to Creat a Blog
      subtitle: ''
      text: ''
      # Page type to display. E.g. post, talk, publication...
      page_type: post
      # Choose how many pages you would like to display (0 = all pages)
      count: 2
      # Filter on criteria
      filters:
        author: ""
        category: ""
        tag: ""
        exclude_featured: false
        exclude_future: false
        exclude_past: false
        publication_type: ""
      # Choose how many pages you would like to offset by
      offset: 0
      # Page order: descending (desc) or ascending (asc) date.
      order: desc
    design:
      # Choose a layout view
      view: date-title-summary
      # Reduce spacing
      spacing:
        padding: [0, 0, 0, 0]
  - block: cta-card
    demo: true # Only display this section in the Hugo Blox Builder demo site
    content:
      title: 👉 Israel Gebre
        - Data Science
        - Machine Learning
        - Remote Sensing and GIS
        - Geospatial Data Science
        - Environmental Sustainability
      #button:
        #text: Get Started
        #url: https://hugoblox.com/templates/
    design:
      card:
        # Card background color (CSS class)
        css_class: "bg-primary-700"
        css_style: ""
---
