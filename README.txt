README: Real-Time Probability of Scoring Model

1. Overview
-----------
This project shows how to build a probability-of-scoring (similar to an xG model) in real time, using (simulated) tracking data for a soccer/football scenario. Key steps include:

  1. Simulating or loading tracking data (positions of the ball, attackers, and defenders).
  2. Engineering features (distance to goal, number of nearby defenders, etc.).
  3. Training a machine learning model (Logistic Regression) to predict the probability of a goal occurring within the next 5 seconds.
  4. Running a real-time loop to demonstrate how the model could trigger camera actions (e.g., "zoom in" or "start recording").

Note: The code uses simulated data for demonstration. In a real scenario, you would replace the simulation with actual tracking data (e.g., from optical tracking or a live feed).


2. Files
--------
- ModelScoringGoal.ipynb or vertopal.com_ModelScoringGoal.txt: Python code.
- README.md (this file): Documentation explaining usage and workflow.


3. Requirements
---------------
- Python 3.7+
- Libraries:
  - numpy
  - pandas
  - scikit-learn
  - time (built-in)

Install them via:
  pip install numpy pandas scikit-learn


4. Usage
--------
1. Open the notebook ModelScoringGoal.ipynb (or run the .txt script) in a Python environment or Jupyter notebook.
2. Run the cells step by step:
   - Simulate tracking data or load your own real data.
   - Train the Logistic Regression model on historical data.
   - Evaluate it via AUC and Brier score.
   - Run the "real-time inference simulation" to see how the model might operate in an actual match.


5. Workflow in the Code
-----------------------
1. Data Simulation (simulate_tracking_data):
   - Creates random positions for the ball, three attackers, and three defenders on a 105×68 pitch.
   - Labels each record with goal_next_5s=1 if conditions for a "goal" are met (e.g., ball near goal, few defenders close).

2. Feature Extraction (extract_features):
   - Calculates distance_to_goal (ball to goal center).
   - Counts defenders_close (defenders within 10 meters).
   - Computes avg_attacker_dist from ball to attackers.

3. Model Training (train_model):
   - Splits data into train/test.
   - Trains a Logistic Regression to predict goal_next_5s.
   - Prints AUC and Brier Score to measure performance.

4. Real-Time Inference (run_real_time_inference):
   - Simulates a loop that repeatedly generates new data points (like live frames).
   - Extracts features, gets a probability of an imminent goal, and if above a threshold, triggers camera actions (set_camera_focus, start_highlight_recording).


6. Example Output
----------------
After training on ~2000 simulated samples, you might see:

  Training model...
  Model performance on test set:
    AUC = 0.983
    Brier Score = 0.039

  Entering real-time inference simulation...

  Time step 1: Probability of goal/danger in next 5s = 0.000

  Time step 2: Probability of goal/danger in next 5s = 0.995
  [CAMERA] Switching to focus mode: close-up
  [CAMERA] Starting highlight recording...

  Time step 3: Probability of goal/danger in next 5s = 0.974
  [CAMERA] Switching to focus mode: close-up
  [CAMERA] Starting highlight recording...

  ...

AUC = 0.983 indicates the model can almost perfectly distinguish "goal likely soon" vs. "not likely."
Brier Score = 0.039 indicates good calibration (lower is better).
Whenever probability exceeds the threshold (0.3 in the example), the camera is triggered.


7. Interpreting the Results
---------------------------
- AUC measures how well the model differentiates positive (goal in next 5s) from negative (no goal). Ranges from 0.5 (random) to 1.0 (perfect).
- Brier Score measures calibration (difference between predicted probabilities and actual outcomes). 0 is perfect, 1 is worst.
Because this demo uses simulated data, results can be quite high. With real data, AUC and calibration may be lower due to real-world complexity.


8. Customization
----------------
- More Players: Extend the simulation or real data to include 22 player coordinates.
- Advanced Models: Try XGBoost or neural networks for better performance.
- Real Data: Replace the simulation with actual tracking (x,y) positions from a match feed or commercial provider.


9. License and Credits
----------------------
- Code is provided under an open-source license (MIT or your preference).
- Simulation logic is for demonstration—no real match data is included.
- For real data usage, ensure you have the rights/permissions from the data provider.


