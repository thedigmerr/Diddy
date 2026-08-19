# Diddy
Gbm-simulation
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import ipywidgets as widgets

def interactive_gbm(n_years=10, n_scenarios=100, mu=0.07, sigma=0.15, s_0=100):
    dt = 1 / 12
    n_steps = int(n_years / dt)
    rets = np.random.normal(loc=mu * dt, scale=sigma * np.sqrt(dt), size=(n_steps, n_scenarios))
    ret_val = np.vstack([np.ones(n_scenarios), 1 + rets])
    prices = pd.DataFrame(s_0 * ret_val.cumprod(axis=0))

    fig, (ax_prices, ax_hist) = plt.subplots(nrows=1, ncols=2, sharey=True, gridspec_kw={'width_ratios': [3, 1]}, figsize=(12, 6))
    plt.subplots_adjust(wspace=0.0)

    prices.plot(ax=ax_prices, legend=False, alpha=0.5, color="skyblue")
    ax_prices.axhline(y=s_0, ls=":", color="black")
    ax_prices.set_xlabel("Years")
    ax_prices.set_ylabel("Price")

    terminal_values = prices.iloc[-1]
    terminal_values.plot.hist(ax=ax_hist, bins=50, orientation="horizontal", color="steelblue")
    ax_hist.axhline(y=s_0, ls=":", color="black")
    ax_hist.set_xlabel("Distribution")

    plt.show()

widgets.interactive(interactive_gbm, n_years=(1, 30), n_scenarios=(1, 1000), mu=(-0.1, 0.1, 0.01), sigma=(0, 0.3, 0.01), s_0=(100, 1000, 10))
