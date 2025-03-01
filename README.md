import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
import numpy as np

# Import data
df = pd.read_csv("medical_examination.csv")

# Add overweight column (BMI > 25 is overweight)
df["BMI"] = df["weight"] / ((df["height"] / 100) ** 2)
df["overweight"] = (df["BMI"] > 25).astype(int)
df.drop(columns=["BMI"], inplace=True)  # Remove temporary BMI column

# Normalize cholesterol and glucose (1 -> 0, else -> 1)
df["cholesterol"] = (df["cholesterol"] > 1).astype(int)
df["gluc"] = (df["gluc"] > 1).astype(int)

# Draw Categorical Plot
def draw_cat_plot():
    # Melt data for categorical plot
    df_cat = pd.melt(df, id_vars=["cardio"], value_vars=["cholesterol", "gluc", "smoke", "alco", "active", "overweight"])

    # Group and count values
    df_cat = df_cat.groupby(["cardio", "variable", "value"]).size().reset_index(name="total")

    # Create catplot
    fig = sns.catplot(
        x="variable", y="total", hue="value", col="cardio",
        data=df_cat, kind="bar", height=5
    ).fig

    return fig

# Draw Heat Map
def draw_heat_map():
    # Clean data based on specified conditions
    df_heat = df[
        (df["ap_lo"] <= df["ap_hi"]) &
        (df["height"] >= df["height"].quantile(0.025)) &
        (df["height"] <= df["height"].quantile(0.975)) &
        (df["weight"] >= df["weight"].quantile(0.025)) &
        (df["weight"] <= df["weight"].quantile(0.975))
    ]

    # Compute correlation matrix
    corr = df_heat.corr()

    # Generate a mask for the upper triangle
    mask = np.triu(np.ones_like(corr, dtype=bool))

    # Set up the matplotlib figure
    fig, ax = plt.subplots(figsize=(10, 8))

    # Draw the heatmap
    sns.heatmap(corr, annot=True, fmt=".1f", mask=mask, cmap="coolwarm", linewidths=0.5, ax=ax)

    return fig
