Pine Script v6 Documentation: A Comprehensive Guide
This document provides a detailed overview of Pine Script v6, the programming language used on TradingView for creating custom indicators and trading strategies.

1. Basic Structure of a Pine Script
Every Pine Script starts with the version declaration and a script type definition.

//@version=6
indicator("My First Indicator", overlay=true) // Or strategy("My First Strategy", overlay=true)

// Script logic goes here

//@version=6: Specifies the Pine Script version. Always start your script with this.

indicator(): Defines an indicator.

"My First Indicator": The name that will appear on the chart.

overlay=true: If true, the indicator plots directly on the price chart. If false, it plots in a separate pane below the chart.

strategy(): Defines a trading strategy.

"My First Strategy": The name that will appear on the chart.

overlay=true: Similar to indicator(), determines if strategy plots (entries/exits) appear on the price chart.

2. Variables and Types
Pine Script is a strongly typed language. Variables must be declared with their type (though often inferred) and cannot change type after declaration.

Common Types:
int: Integer numbers (e.g., 10, -5).

float: Floating-point numbers (e.g., 10.5, 3.14159).

bool: Boolean values (true or false).

string: Text (e.g., "Hello World").

color: Color values (e.g., color.red, color.new(#FF0000, 50)).

series: A series of values (e.g., close, high, ta.ema(close, 20)). This is the most common type for price data and indicator outputs.

array<type>: An array of a specific type (e.g., array<float>, array<bool>).

line, label, table: Objects for drawing on the chart.

Variable Declaration:
Use var, varip, or implicit declaration. const is for compile-time constants.

// Implicit declaration (type inferred)
myValue = close + open

// Explicit declaration with 'var' (initialized once on the first bar)
var float myFloat = na // 'na' means Not Available, useful for initial state

// 'varip' (initialized once, persists across script recompilations)
varip int counter = 0

// Constant
const string MY_CONSTANT = "Hello"

3. Operators
Pine Script supports standard arithmetic, comparison, logical, and ternary operators.

Arithmetic: +, -, *, /, % (modulo)

Comparison: == (equal to), != (not equal to), > (greater than), < (less than), >= (greater than or equal to), <= (less than or equal to)

Logical: and, or, not

Ternary: condition ? value_if_true : value_if_false

isBullish = close > open
emaCross = ta.crossover(ta.ema(close, 10), ta.ema(close, 20))
signalColor = isBullish ? color.green : color.red

4. Built-in Functions
Pine Script provides a rich set of built-in functions for technical analysis, plotting, and more.

ta Library (Technical Analysis):
The ta namespace contains most common technical indicators.

ema20 = ta.ema(close, 20) // 20-period Exponential Moving Average of close price
rsi14 = ta.rsi(close, 14) // 14-period Relative Strength Index
atrValue = ta.atr(14)   // 14-period Average True Range

Plotting Functions:
plot(): Plots a series on the chart.

plotshape(): Plots a shape (e.g., arrow, circle) on the chart.

plotchar(): Plots a Unicode character on the chart.

bgcolor(): Colors the background of the chart.

hline(): Plots a horizontal line.

plot(ema20, title="EMA 20", color=color.blue, linewidth=2)
plotshape(rsi14 < 30, title="RSI Oversold", style=shape.triangleup, location=location.belowbar, color=color.green)
bgcolor(rsi14 > 70 ? color.new(color.red, 90) : na) // Light red background for overbought

Drawing Objects:
line.new(): Creates a new line object.

label.new(): Creates a new label object.

table.new(): Creates a new table object for displaying data.

var line myLine = na
if barstate.islast
    myLine := line.new(bar_index[10], close[10], bar_index, close, color=color.purple, width=2)
    label.new(bar_index, high, text="High Bar", yloc=yloc.abovebar, style=label.style_label_down, color=color.blue)

5. User-Defined Functions
You can create your own functions to encapsulate reusable logic.

// Function to calculate a custom average
f_customAvg(source, length) =>
    sum = 0.0
    for i = 0 to length - 1
        sum += source[i]
    sum / length

myCustomAvg = f_customAvg(close, 10)
plot(myCustomAvg, title="Custom Avg")

6. Conditional Logic
Use if, else if, and else for conditional execution.

if close > open
    // Price increased
    plotcolor = color.green
else if close < open
    // Price decreased
    plotcolor = color.red
else
    // Price unchanged
    plotcolor = color.gray

plot(close, color=plotcolor)

7. Loops
While direct for loops over historical bars are generally discouraged for performance (Pine Script processes bar by bar), you can use for loops for array manipulation or specific calculations within a single bar's context.

// Example: Summing elements in an array
myArray = array.from(1, 2, 3, 4, 5)
sum = 0
for i = 0 to array.size(myArray) - 1
    sum += array.get(myArray, i)
// sum will be 15

8. Strategies
Strategies allow you to simulate trades based on your indicator signals.

strategy.entry(): Enters a long or short position.

strategy.exit(): Exits a position with optional profit target and stop loss.

strategy.close(): Closes an open position.

strategy.risk.allow_intra_bar_orders(): Allows orders to be filled on the same bar they are generated.

longCondition = ta.crossover(ta.ema(close, 10), ta.ema(close, 20))
shortCondition = ta.crossunder(ta.ema(close, 10), ta.ema(close, 20))

if longCondition
    strategy.entry("Long", strategy.long)

if shortCondition
    strategy.close("Long") // Close existing long position
    strategy.entry("Short", strategy.short) // Enter short position

Strategy Properties:
Use strategy() function arguments to configure strategy properties.

strategy(
  "My Strategy",
  overlay=true,
  initial_capital=10000,
  default_qty_type=strategy.percent_of_equity,
  default_qty_value=10,
  commission_type=strategy.commission.percent,
  commission_value=0.1,
  pyramiding=0 // No pyramiding
)

9. Libraries
Pine Script v5 introduced libraries, allowing you to create reusable modules of code.

Creating a Library:
Save a script with library() declaration.

//@version=6
library("MyMathLib", overlay=false)

export add(a, b) =>
    a + b

export subtract(a, b) =>
    a - b

Importing a Library:
Use the import statement in your indicator/strategy.

//@version=6
indicator("Using My Library", overlay=true)

import MyUsername/MyMathLib/1 as mylib // Replace MyUsername with the actual username and 1 with version

result = mylib.add(10, 5)
plot(result)

10. request.security() for Multi-Timeframe Analysis
request.security() allows you to fetch data from different timeframes or symbols. It's crucial to use it correctly to avoid look-ahead bias.

// Get daily close price
dailyClose = request.security(syminfo.tickerid, "D", close)

// Get EMA from a higher timeframe without look-ahead bias
// `barmerge.gaps_on` and `barmerge.lookahead_on` are important for proper handling
htf_ema = request.security(syminfo.tickerid, "60", ta.ema(close, 20)[barstate.isconfirmed ? 0 : 1], barmerge.gaps_on, barmerge.lookahead_on)

plot(htf_ema, title="HTF EMA")

syminfo.tickerid: Current symbol.

"D": Daily timeframe. "60": 60-minute timeframe.

close: The series to request.

[barstate.isconfirmed ? 0 : 1]: This is a critical part to prevent look-ahead bias. It ensures that on the current (unconfirmed) bar, you are looking at the previous bar's value from the higher timeframe.

barmerge.gaps_on: Fills gaps with na if data is missing.

barmerge.lookahead_on: Allows the requested data to be available at the current bar's close, preventing look-ahead bias when used with [1] or [barstate.isconfirmed ? 0 : 1].

11. Debugging Tips
plot() and label.new(): Plot variables you want to inspect. Use labels to display values at specific points on the chart.

log.info(): Print messages to the Pine Script console (visible when adding the script to the chart).

na and nz(): Handle na (Not Available) values. nz(value, default_value) replaces na with a default.

Step-by-step execution: Understand how Pine Script processes data bar by bar.

12. Best Practices
Modularity: Break down complex logic into smaller, reusable functions.

Readability: Use clear variable names and comments for complex sections.

Performance: Avoid unnecessary calculations or loops. Be mindful of how request.security() is used.

No Repainting: Ensure your signals do not change on historical bars after they have been confirmed. Always consider barstate.isconfirmed and proper request.security() usage.

Error Handling: Use na checks and nz() where appropriate.

Input Management: Use input.* functions to allow users to customize parameters.

// Example of input
emaLength = input.int(20, title="EMA Length", minval=1)

This documentation should provide a solid foundation for coding effectively in Pine Script v6!