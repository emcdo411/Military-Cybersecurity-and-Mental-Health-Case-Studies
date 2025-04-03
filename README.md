# Military Cybersecurity and Mental Health Case Studies

This repository contains two interrelated case studies that explore important aspects of modern security and mental health issues faced by the Indian Military. The first case study deals with the visualization of cyberattack frequencies in India, China, and Pakistan, providing insights into the increasing cybersecurity threats. The second case study focuses on the mental health challenges faced by Indian military personnel, specifically dealing with Post-Traumatic Stress Disorder (PTSD).

## Table of Contents

- [Project Overview](#project-overview)
- [Cyberattack Frequency Case Study](#cyberattack-frequency-case-study)
  - [Data Preparation](#data-preparation)
  - [Shiny App Implementation](#shiny-app-implementation)
  - [Map of Cyberattack Frequency](#map-of-cyberattack-frequency)
- [Indian Military PTSD Data Case Study](#indian-military-ptsd-data-case-study)
  - [Data Preparation](#data-preparation-2)
  - [Visualizing PTSD Data](#visualizing-ptsd-data)
- [Why This Matters](#why-this-matters)
- [Conclusion](#conclusion)

---

## Project Overview

This repository contains two case studies that look at different aspects of security and mental health in the context of the Indian Military:

1. **Cyberattack Frequency Case Study**: This part of the project visualizes the frequency of cyberattacks on India, China, and Pakistan, providing valuable insights into the growing threats in the digital world.
2. **Indian Military PTSD Case Study**: The second part focuses on the mental health challenges faced by Indian military personnel, particularly the prevalence of PTSD, and how data can help inform better support systems for military personnel.

Both case studies use R programming language and Shiny applications to visualize and analyze the data, providing a deeper understanding of how these issues impact military operations and personnel.

---

## Cyberattack Frequency Case Study

### Data Preparation

In this case study, we analyze the frequency of cyberattacks between India, China, and Pakistan from 2015 to 2020. The data consists of various types of cyberattacks such as espionage attacks, infrastructure attacks, and data theft. Here's the process of preparing the data for visualization:

```r
# Load necessary libraries
library(dplyr)
library(tidyr)

# Simulate data for the Cyberattack Frequency Case Study
cyberattack_data <- data.frame(
  Year = rep(2015:2020, each = 3),  # 3 countries, 6 years
  Country = rep(c("India", "China", "Pakistan"), times = 6),  # Three countries
  Total_Cyberattacks = sample(100:500, 18, replace = TRUE),  # Random number of total cyberattacks
  Espionage_Attacks = sample(20:100, 18, replace = TRUE),  # Random number of espionage attacks
  Infrastructure_Attacks = sample(10:50, 18, replace = TRUE),  # Random number of infrastructure attacks
  Data_Theft_Attacks = sample(5:30, 18, replace = TRUE),  # Random number of data theft attacks
  Latitude = rep(c(28.6139, 39.9042, 33.6844), times = 6),  # Latitude for India, China, Pakistan
  Longitude = rep(c(77.2090, 116.4074, 73.0479), times = 6)  # Longitude for India, China, Pakistan
)

# Reshape the data for easier plotting
cyberattack_data_long <- cyberattack_data %>%
  pivot_longer(cols = c("Total_Cyberattacks", "Espionage_Attacks", "Infrastructure_Attacks", "Data_Theft_Attacks"),
               names_to = "Attack_Type",
               values_to = "Frequency")
```

### Shiny App Implementation

The Shiny app allows us to visualize the frequency of cyberattacks on an interactive map. Users can filter the data by year and country to see specific attack types. Here’s the UI and server code for the app:

```r
# Define the UI
ui <- fluidPage(
  titlePanel("Cyberattack Frequency Map"),
  sidebarLayout(
    sidebarPanel(
      selectInput("year", "Select Year:", choices = unique(cyberattack_data_long$Year), selected = 2015),
      selectInput("country", "Select Country:", choices = unique(cyberattack_data_long$Country), selected = "India")
    ),
    mainPanel(
      leafletOutput("map")
    )
  )
)

# Define the server logic
server <- function(input, output) {
  filtered_data <- reactive({
    cyberattack_data_long %>% filter(Year == input$year & Country == input$country)
  })
  
  output$map <- renderLeaflet({
    data <- filtered_data()
    leaflet(data) %>%
      addTiles() %>%
      addMarkers(
        lat = ~Latitude, lng = ~Longitude,
        popup = ~paste(
          "<strong>Country:</strong>", Country, "<br>",
          "<strong>Year:</strong>", Year, "<br>",
          "<strong>Attack Type:</strong>", Attack_Type, "<br>",
          "<strong>Frequency:</strong>", Frequency
        )
      )
  })
}

# Run the application
shinyApp(ui = ui, server = server)
```

### Map of Cyberattack Frequency

The resulting Shiny app allows users to explore cyberattack data interactively by selecting different years and countries. The map displays markers with popups that show the frequency of different types of cyberattacks. 

---

## Indian Military PTSD Data Case Study

### Data Preparation

In this case study, we focus on the mental health challenges faced by the Indian military, particularly PTSD. The dataset includes demographic data such as age, years of service, and diagnosis of PTSD in military personnel.

```r
# Simulated data for PTSD in the Indian Military
ptsd_data <- data.frame(
  Age = c(25, 30, 35, 40, 45, 50),
  Years_of_Service = c(2, 5, 8, 10, 12, 15),
  PTSD_Diagnosis = c("Yes", "No", "Yes", "No", "Yes", "Yes"),
  Number_of_Cases = c(10, 5, 20, 15, 30, 25)
)
```

### Visualizing PTSD Data

While we are excluding the bar graph plot code from the readme, we can explain that the data visualizations will focus on understanding the correlation between age, years of service, and the prevalence of PTSD in the Indian military.

---

## Why This Matters

The intersection of cybersecurity and mental health in military personnel is an area that deserves attention. As technology becomes more integral to modern warfare, the threat of cyberattacks continues to rise, especially in regions with geopolitical tensions. In countries like India, where cyberattacks from neighboring countries (China and Pakistan) are prevalent, the military must be equipped to face these digital threats.

On the mental health side, Post-Traumatic Stress Disorder (PTSD) remains a critical issue for military personnel. Many soldiers and officers experience traumatic events that affect their mental health and overall well-being. Understanding the prevalence of PTSD and finding effective ways to support those affected is vital for ensuring a capable and resilient military force.

---

## Conclusion

In this repository, we explored two important issues:
1. **Cybersecurity Threats**: We created an interactive Shiny app that visualizes the frequency of cyberattacks targeting India, China, and Pakistan. This highlights the importance of cybersecurity awareness and preparedness in the military.
2. **Mental Health in the Military**: We simulated PTSD data to explore the mental health challenges faced by Indian military personnel, shedding light on how data can help inform policies and support systems for those affected.

By combining these two case studies, we underscore the importance of both physical and mental well-being in ensuring the readiness and effectiveness of military forces.

---

Feel free to explore the code, run the Shiny app, and modify the data to fit your own needs. This repository provides a starting point for future analysis on military cybersecurity and mental health.

---

### **To get started:**
1. Clone the repository to your local machine.
2. Install the necessary libraries in R (`shiny`, `leaflet`, `dplyr`, `tidyr`).
3. Run the Shiny app and interact with the visualizations.

---

This README provides an overview of both case studies and includes all relevant code, minus the bar plot visualization code. Let me know if you need further tweaks!
