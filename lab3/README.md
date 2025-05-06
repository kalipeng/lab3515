**Discussion**

After testing our sorting hat with 10 questions, I found that certain questions contribute significantly more to accurate house predictions than others. Questions exploring values and decision-making (like Q1 and Q2) provide deeper personality insights than superficial preference questions such as "favorite subject" (Q3) or "preferred pet" (Q7). From a user experience perspective, removing Q7 and Q10 would streamline the interaction without sacrificing predictive quality.

The technical implementation could be improved in two key ways. First, replacing our basic decision tree with either a Random Forest or KNN model would enhance prediction accuracy while remaining lightweight enough for our embedded system. Second, integrating sensors (microphone, touch sensors, or biometric readers) would transform the experience from a questionnaire to something genuinely magical and intuitive.

With sensor integration, we would need to upgrade from our simple decision tree to either a small neural network or logistic regression model to properly handle the continuous data streams. This approach would better capture subtle personality indicators beyond self-reported answers, creating an experience closer to the sorting hat's fictional mind-reading capabilities.

I'm curious whether we should prioritize refining our question set first or move directly toward sensor enhancement for a more immersive experience.


# Brief Documentation of Implementation

This project implements a **Harry Potter-style sorting hat** on an **ESP32-WROOM-32** board using machine learning.

### 1. Dataset & Model Training

I created a dataset of more than 30 student responses, each consisting of 10 multiple-choice answers and a corresponding Hogwarts house label (`Gryffindor`, `Hufflepuff`, `Ravenclaw`, `Slytherin`).
Using Python and `scikit-learn`, I trained a **DecisionTreeClassifier** and exported the model to C++ format using `micromlgen`. The final model was saved as `sorting_hat_model.h`.

### 2. Hardware Setup

* **ESP32 DevKit** (WROOM-32)
* **4 Push Buttons** connected to GPIOs 18, 17, 16, and 4
* **OLED Display (SSD1306 128x64)** connected via I2C (SDA: GPIO 21, SCL: GPIO 22)
  All components were powered via USB and wired on a breadboard.

### 3. Arduino Implementation

The Arduino sketch:

* Initializes the OLED and button inputs
* Displays each question and 4 options
* Waits for a button press, records the response
* After 10 questions, runs the decision tree model on-device to predict the house
* Displays the final house result on the OLED screen
