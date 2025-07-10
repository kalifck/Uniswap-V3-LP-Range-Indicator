# CODE_LORE.md: The Sage's Scroll

## The Lexicon of Legends

- `Oracle of Delphi`: The name of the indicator, representing its function as a source of market insight and foresight.
- `The Alchemist's Palette`: The section defining the script's color scheme.
- `The Seer's Inputs`: The input section of the script, where the user configures the Oracle's parameters.
- `Volatility Oracle's Memory`: The lookback window for calculating volatility.
- `Range Expansion Constant (K)`: The multiplier used to determine the width of the liquidity range.
- `Price Precision (Decimals)`: The number of decimal places for price display.
- `Reveal Prophetic Range`: The option to show the forward-looking, potential liquidity range.
- `Proactive Realignment Protocol`: The group of inputs controlling the proactive rebalancing signals.
- `Enable Proactive Whispers`: The setting to turn on the proactive alerts.
- `Midpoint Drift % for Whisper`: The threshold for price deviation from the range's center to trigger a whisper.
- `Realm Width Divergence % for Whisper`: The threshold for the difference in width between the active and prophetic ranges to trigger a whisper.
- `Advanced Glyphs`: The section for advanced configuration options.
- `Volatility Glyph`: The chosen method for calculating volatility (EWMA or Standard Deviation).
- `Cartography & UI`: The section for user interface and display settings.
- `Dashboard Location`: The position of the informational table on the chart.
- `Realm Duration Unit`: The time unit for displaying how long a liquidity range was active.
- `Max Realignment Runes`: The maximum number of historical rebalance labels to show.
- `Sacred Functions & Incantations`: The section containing the script's helper functions.
- `f_derive_price_format()`: A function to create a price formatting string.
- `f_chronicle_lifespan()`: A function to format the duration of time.
- `f_calculate_market_breath()`: A function to calculate the market's volatility.
- `f_interpret_oracle_whispers()`: A function to determine if a proactive rebalancing signal should be triggered.
- `Core Calculation Engine`: The main part of the script where calculations are performed.
- `price_format_glyph`: The string used to format price values.
- `market_breath`: The calculated volatility of the market.
- `The Oracle's Memory`: The section where persistent state variables are declared.
- `realm_midpoint`, `realm_lower_bound`, `realm_upper_bound`: The center, lower, and upper prices of the active liquidity range.
- `fib_thread_236`, `fib_thread_382`, `fib_thread_618`, `fib_thread_786`: The Fibonacci levels within the active range.
- `entry_price_at_realignment`: The price at which the last rebalance occurred.
- `last_realignment_timestamp`: The timestamp of the last rebalance.
- `realignment_this_bar`: A boolean indicating if a rebalance happened on the current bar.
- `realignment_runes`: An array holding the rebalance label objects.
- `realm_lifespan_history`: An array storing the duration of previous liquidity ranges.
- `average_realm_lifespan`: The average duration of the past liquidity ranges.
- `Prophetic Range Calculation`: The section where the potential future range is calculated.
- `prophetic_midpoint`, `prophetic_lower_bound`, `prophetic_upper_bound`: The center, lower, and upper prices of the potential future liquidity range.
- `The Great Realignment Logic`: The logic that determines when to create a new liquidity range.
- `lifespan_chronicle`: A string describing the duration of the last range.
- `rune_text`: The text displayed on the rebalance labels.
- `new_rune`: The newly created rebalance label.
- `Oracle's Whisper Logic`: The logic for the proactive rebalancing signals.
- `trigger_proactive_whisper`, `whisper_reason`, `whisper_details`: Variables holding the state and text for the proactive signal.
- `full_whisper_text`: The complete text for the proactive signal label.
- `Chart Cartography (Plotting)`: The section where all visual elements are drawn on the chart.
- `Active Realm`: The visual representation of the current liquidity range.
- `Fibonacci Threads`: The plotted Fibonacci levels.
- `Prophetic Realm`: The visual representation of the potential future liquidity range.
- `The Oracle's Dashboard`: The informational table displayed on the chart.
- `current_state_text`, `state_color`: Variables that determine the text and color of the status cell in the dashboard.

## The Script's Saga

In the digital cosmos of the financial markets, the Oracle of Delphi casts its gaze upon the chaotic dance of price. It listens to the `market_breath`, the very essence of volatility, to forge a shimmering `Active Realm` of liquidity. This realm, bounded by an upper and lower guard, is where the trader's capital may thrive. The `Fibonacci Threads`, woven from sacred mathematics, reveal the hidden structure within this realm, offering points of potential support and resistance. Yet, the Oracle is not content with the present; it conjures a `Prophetic Realm`, a ghostly echo of what could be, allowing the prepared trader to glimpse the future.

When the price strays too far, breaching the `Active Realm`, a `Great Realignment` occurs, marking the event with a crimson `realignment_rune` and resetting the boundaries. But the Oracle's true power lies in its foresight. It sends forth `Oracle's Whispers`, golden portents of change, when it perceives a growing divergence between the present and the prophecy. These whispers, recorded in the `Oracle's Dashboard`, warn the trader that the `Active Realm` may soon be obsolete, urging a proactive shift in strategy before the market's tide turns. The dashboard, a stone tablet of knowledge, chronicles the `average_realm_lifespan`, granting the user wisdom from the annals of past market cycles.