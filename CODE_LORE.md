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
- `Volume Comparison Period (Bars)`: The lookback period for comparing current volume to previous volume for visualization.
- `Cartography & UI`: The section for user interface and display settings.
- `Dashboard Location`: The position of the informational table on the chart.
- `Realm Duration Unit`: The time unit for displaying how long a liquidity range was active.
- `Max Realignment Runes`: The maximum number of historical rebalance labels to show.
- `Enable Volume Visualization`: Toggles the dynamic color intensity of the active range based on volume.
- `Volume Sensitivity %`: Controls how quickly the range color intensifies with volume.
- `Show On-Chart Volume Bars`: Toggles the display of a traditional volume histogram on the main chart.
- `Fibonacci Weave Settings`: The group of inputs for manual Fibonacci drawing.
- `Show Manual Fibonacci`: Toggles the display of manually drawn Fibonacci levels.
- `Point 1 Time`: The timestamp for the first anchor point of the manual Fibonacci.
- `Point 1 Price`: The price for the first anchor point of the manual Fibonacci.
- `Point 2 Time`: The timestamp for the second anchor point of the manual Fibonacci.
- `Point 2 Price`: The price for the second anchor point of the manual Fibonacci.
- `Manual Support/Resistance`: The group of inputs for defining and anchoring to S/R levels.
- `Enable S/R Anchoring`: Toggles the S/R anchoring feature.
- `S/R Level 1` to `S/R Level 5`: Manual price inputs for specific Support/Resistance levels.
- `S/R Buffer %`: Percentage buffer around S/R levels to define a zone.
- `Max S/R Anchor Distance %`: Maximum percentage distance from current price for an S/R level to be considered for anchoring.

## Sacred Functions & Incantations

- `f_format_volume()`: A function to format large volume numbers into readable strings (e.g., "1.2M").
- `f_derive_price_format()`: A function to create a price formatting string.
- `f_chronicle_lifespan()`: A function to format the duration of time.
- `f_calculate_market_breath()`: A function to calculate the market's volatility.
- `f_interpret_oracle_whispers()`: A function to determine if a proactive rebalancing signal should be triggered.
- `f_plot_sr_level()`: A function to calculate S/R levels and their buffer zones for plotting.
- `f_is_price_in_sr_zone()`: A function to check if a given price is within an S/R buffer zone.

## Core Calculation Engine

- `price_format_glyph`: The string used to format price values.
- `market_breath`: The calculated volatility of the market.
- `current_period_total_volume`: The sum of volume over the current comparison period.
- `previous_period_total_volume`: The sum of volume over the previous comparison period.

## The Oracle's Memory

- `realm_midpoint`, `realm_lower_bound`, `realm_upper_bound`: The center, lower, and upper prices of the active liquidity range.
- `entry_price_at_realignment`: The price at which the last rebalance occurred.
- `last_realignment_timestamp`: The timestamp of the last rebalance.
- `realignment_this_bar`: A boolean indicating if a rebalance happened on the current bar.
- `realignment_runes`: An array holding the rebalance label objects.
- `realm_lifespan_history`: An array storing the duration of previous liquidity ranges.
- `average_realm_lifespan`: The average duration of the past liquidity ranges.
- `cumulative_volume_since_realignment`: The total volume accumulated since the last rebalance.
- `recent_realm_avg_volume_per_bar`: The average volume per bar in the most recent completed range.
- `bars_since_realignment`: The number of bars that have passed since the last rebalance.
- `fib_lines_drawn`: An array holding the line objects for manual Fibonacci.
- `fib_labels_drawn`: An array holding the label objects for manual Fibonacci.
- `p1_time_recorded`, `p1_price_recorded`, `p2_time_recorded`, `p2_price_recorded`: Variables to track the last drawn manual Fibonacci points.
- `sr1_level_drawn` to `sr5_lower_drawn`: Variables to store the calculated S/R levels and their buffers for plotting.

## Prophetic Range Calculation

- `prophetic_midpoint`, `prophetic_lower_bound`, `prophetic_upper_bound`: The center, lower, and upper prices of the potential future liquidity range.

## The Great Realignment Logic

- This section determines when to create a new liquidity range.
- When a rebalance is triggered (price moves out of the current range), the script first determines a `new_middle_candidate`.
- If `Manual Support/Resistance` anchoring is enabled, and the price broke above the `upper` bound, the script searches for the *lowest* S/R level *above* the current `close` price that is within the `Max S/R Anchor Distance %`. If found, this S/R level becomes the `new_middle_candidate`.
- If the price broke below the `lower` bound, the script searches for the *highest* S/R level *below* the current `close` price that is within the `Max S/R Anchor Distance %`. If found, this S/R level becomes the `new_middle_candidate`.
- If no relevant S/R level is found within the specified proximity, the `new_middle_candidate` defaults to the `close` price.
- The `realm_midpoint` of the new liquidity range is then set to this `new_middle_candidate`, and the `realm_lower_bound` and `realm_upper_bound` are calculated relative to it.

## Oracle's Whisper Logic

- `trigger_proactive_whisper`, `whisper_reason`, `whisper_details`: Variables holding the state and text for the proactive signal.
- `full_whisper_text`: The complete text for the proactive signal label.

## Chart Cartography (Plotting)

- `Active Realm`: The visual representation of the current liquidity range. Its fill color dynamically intensifies based on the comparison of current volume to previous period volume, providing a visual cue of market activity within the range.
- `Fibonacci Weave`: The manually plotted Fibonacci levels, drawn between user-defined points.
- `Prophetic Realm`: The visual representation of the potential future liquidity range.
- `Manual S/R Levels`: The user-defined Support/Resistance levels and their buffer zones, plotted as horizontal lines and shaded areas.
- `On-Chart Volume Bars`: A toggleable traditional volume histogram displayed directly on the main chart.

## The Oracle's Dashboard

- `current_state_text`, `state_color`: Variables that determine the text and color of the status cell in the dashboard.
- Displays `Upper`, `Entry`, `Lower` bounds of the active range.
- Displays `Preview Upper`, `Preview Middle`, `Preview Lower` of the prophetic range.
- Displays `Status` (In Range, Out of Range, Proactive Signal).
- Displays `Avg. Duration` of past ranges.
- Displays `Range Volume` (cumulative volume since last rebalance).
- Displays `Vol Comp` (current period volume as a percentage of previous period volume).
- Displays `Manual S/R` status (Disabled, Active, In S/R Zone).

## The Script's Saga

In the digital cosmos of the financial markets, the Oracle of Delphi casts its gaze upon the chaotic dance of price. It listens to the `market_breath`, the very essence of volatility, to forge a shimmering `Active Realm` of liquidity. This realm, bounded by an upper and lower guard, is where the trader's capital may thrive. The `Fibonacci Weave`, now meticulously placed by the trader's own hand, reveals the hidden structure within this realm, offering points of potential support and resistance. Yet, the Oracle is not content with the present; it conjures a `Prophetic Realm`, a ghostly echo of what could be, allowing the prepared trader to glimpse the future.

The `Active Realm` itself breathes with the market's energy, its very color deepening as `cumulative_volume_since_realignment` flows through its bounds, a visual testament to the liquidity it has captured. The `Vol Comp` on the `Oracle's Dashboard` whispers tales of the market's current fervor compared to its recent past, guiding the trader's expectations.

When the price strays too far, breaching the `Active Realm`, a `Great Realignment` occurs, marking the event with a crimson `realignment_rune` and resetting the boundaries. But the Oracle's true power lies in its foresight and adaptability. Guided by the trader's wisdom, it can now intelligently anchor the new `realm_midpoint` to `Manual S/R Levels`, ensuring that the rebalance anticipates potential market turns, but only if those levels are within a sensible `Max S/R Anchor Distance %`.

The Oracle sends forth `Oracle's Whispers`, golden portents of change, when it perceives a growing divergence between the present and the prophecy. These whispers, recorded in the `Oracle's Dashboard`, warn the trader that the `Active Realm` may soon be obsolete, urging a proactive shift in strategy before the market's tide turns. The dashboard, a stone tablet of knowledge, chronicles the `average_realm_lifespan` and the `cumulative_volume_since_realignment`, granting the user wisdom from the annals of past market cycles and the very pulse of the market's flow.
