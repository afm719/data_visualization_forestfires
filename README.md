# XAI for Forest Fire Risk Assessment

**University of Trieste - Data Visualization Exam 2026**

**Author:** Arahí Fernández Monagas

## Project Description
This project applies eXplainable Artificial Intelligence (XAI) techniques, specifically the SHAP (SHapley Additive exPlanations) framework, to interpret a "black-box" Machine Learning model trained to predict the burned area of forest fires. 

The primary objective of this work is not to optimize the algorithm's predictive performance, but to use data visualization as an algorithmic auditing tool. By mapping complex mathematical weights into intuitive visual channels, this project uncovers the underlying physical biases in the model's learning process.

## Dataset
* **Source:** UCI Machine Learning Repository [Cortez and Morais, 2007].
* **Context:** Meteorological records from the Montesinho Natural Park, Portugal.
* **Target Variable:** `area` (burned hectares). Logarithmically transformed `ln(x+1)` in the code to reduce the extreme right skewness towards zero.
* **Analyzed Attributes:** Temperature (`temp`), Relative Humidity (`RH`), Wind (`wind`), and Rain (`rain`).

## Research Questions
This project addresses two levels of visual explanatory analysis:
1. **Global Analysis:** How do different meteorological factors globally influence the fire risk prediction across the entire population?
2. **Local Explanation (Day 5 Case Study):** What specific weather conditions drove the algorithm's prediction for a concrete historical instance?

## Data to Visual Channel Mapping
To fulfill the course requirements, the visualizations are structured by explicitly translating data attributes into the following visual channels:

### 1. Global Analysis (SHAP Summary Beeswarm Plot)
| Data Attribute | Attribute Type | Visual Channel |
| :--- | :--- | :--- |
| Feature Importance | Nominal / Ordinal | Vertical Position (Y-Axis) |
| SHAP Value (Predictive Impact) | Quantitative | Horizontal Position (X-Axis) |
| Predictor Real Value | Quantitative | Diverging Color Scale (Blue to Red) |
| Sample Density | Quantitative | Vertical Jitter |

### 2. Local Analysis (SHAP Waterfall Plot - Index 5)
| Data Attribute | Attribute Type | Visual Channel |
| :--- | :--- | :--- |
| Base Expected Value | Quantitative | Initial X-Axis Origin |
| Feature Contribution | Quantitative | Horizontal Bar Length |
| Impact Direction (+ / -) | Categorical (Binary) | Color (Red = Positive, Blue = Negative) |
| Final Prediction | Quantitative | Final X-Axis Endpoint |

## Visualizations: Why I Chose Them and What They Reveal
When designing this project, I realized that just predicting forest fires was not enough. I needed to understand exactly how the Random Forest model was thinking. That is why I chose these two specific SHAP visualizations to map the algorithmic decisions into visual channels.

<img width="2285" height="894" alt="shap_summary_plot" src="https://github.com/user-attachments/assets/7fbfe666-0a91-401e-8511-0d3cb6253c3e" />


### Global Explanation: SHAP Summary Beeswarm Plot
I selected the summary beeswarm plot because it provides a complete macro-level overview of the model's behavior. Instead of simply ranking the variables by importance, it maps the actual physical values of the weather data to their predictive impact using a diverging color scale. 

What this plot shows is actually the most critical finding of my analysis. Human logic dictates that extreme heat causes fires. However, if you look at the Temperature row, the model placed the red dots (high temperatures) on the negative side of the axis, meaning they unexpectedly decrease the predicted risk. The blue dots (low temperatures) are pushed to the right, increasing the risk. This counterintuitive behavior proves that the algorithm memorized historical outliers from the dataset. It associated massive fires with cooler days, likely because those specific fires were driven by human factors or extreme winds rather than heat. I included this plot because it perfectly demonstrates how XAI visualizations can audit a model and expose hidden data biases.

<img width="2370" height="990" alt="shap_waterfall_local" src="https://github.com/user-attachments/assets/c4f2cea7-8abc-486d-a51a-ff92c51d854b" />

### Local Explanation: SHAP Waterfall Plot (Day 5)
While the summary plot shows overall trends, I chose the waterfall plot to perform a micro-level autopsy of a single prediction. I wanted to demonstrate how the model arrives at a specific decision step-by-step, minimizing the cognitive load for the reader.

This graph decomposes the prediction for Day 5. It shows what happens when weather variables conflict with each other. Starting from the model's base expected value, we can clearly see that the wind speed pushed the risk up, which is represented by the positive red bar. However, the cooling effects of the temperature and relative humidity overpowered the wind, dragging the final prediction down, represented by the negative blue bars. I chose this visual format because the length and color of the bars make it extremely easy to understand the exact tipping point of the environmental risk for any given day.

## Conclusion
Algorithms can inherit historical correlations that defy basic physics. By intentionally maintaining the model's parameters to reflect these historical outliers, this project demonstrates that XAI visualization tools are mandatory before deploying models in real emergency management. Visual auditing is the only way to expose hidden biases and prevent blind trust in automated systems.

## Execution Instructions
1. Ensure Python 3 is installed along with the libraries listed in `requirements.txt` (pandas, scikit-learn, shap, matplotlib, numpy).
2. The main script is located in the Jupyter Notebook `script.ipynb`.
3. Run the cells sequentially. The notebook will load the data from the `/data` folder, train the Random Forest, calculate the SHAP values, and automatically export the high-resolution visualizations to the `/plots` folder.
