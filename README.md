import pandas as pd
import matplotlib.pyplot as plt
from scipy.stats import linregress

# Import data
df = pd.read_csv("epa-sea-level.csv")

# Define function to draw scatter plot and best-fit lines
def draw_plot():
    # Create scatter plot
    plt.figure(figsize=(10, 6))
    plt.scatter(df["Year"], df["CSIRO Adjusted Sea Level"], label="Data", alpha=0.6)

    # Line of best fit for all data (1880-2050)
    slope1, intercept1, _, _, _ = linregress(df["Year"], df["CSIRO Adjusted Sea Level"])
    years1 = range(1880, 2051)
    plt.plot(years1, intercept1 + slope1 * years1, "r", label="Best Fit (1880-2050)")

    # Line of best fit for data from year 2000-2050
    df_recent = df[df["Year"] >= 2000]
    slope2, intercept2, _, _, _ = linregress(df_recent["Year"], df_recent["CSIRO Adjusted Sea Level"])
    years2 = range(2000, 2051)
    plt.plot(years2, intercept2 + slope2 * years2, "g", label="Best Fit (2000-2050)")

    # Formatting
    plt.xlabel("Year")
    plt.ylabel("Sea Level (inches)")
    plt.title("Rise in Sea Level")
    plt.legend()
    plt.grid(True)

    return plt.gca()
