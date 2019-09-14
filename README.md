# Plot-the-Stock-Price-and-Trading-Volume
To plot the stock price and trading volume to dig out the trading signal

## plot the stock chart with trading volume and stock price 

Just for my own curiosity, I want to create a chart which show how trading volume and trend of tock price are correlated visually. Therefore I tried to build a chart to achieve this function. It's quit hard, just took me an afternoon to finish a all the works.

For some traders who favor technical analysis, they strongly believe trading volume is a good indicator showing the persistence of a upward trend or a downward trend of a stock, in order that they can predict the next move of the market. Certainly, the market is not always working in that way. Otherwise there won't be any investors losing their bets. However, it is still worthy plotting the chart to have a better grip on the market sentiment.

## Code

```python

import matplotlib.pyplot as plt
from pandas_datareader import data

def plot_stock_volumne(stock_quote, start_date, end_date):
    
    '''
    stock_quote: a list of stock quotes you want to plot
        if there is just 1 stock, please enclose it with ""[]
        e.g. ['CRM']
    
    start_date: the starting date of stock price data extracted
        please follow the ISO 0861 standard like 'YYYY-MM-DD'
    
    end_date: the last date of stock price data extracted
        please follow the ISO 0861 standard like 'YYYY-MM-DD'
    '''

    # number of stock in stock_quote
    rows_in_fig = len(stock_quote)
    
    # set axes_counter = 1
    axes_counter = 1
    
    # Add an ax to the figure you just created
    fig, ax = plt.subplots(rows_in_fig, 1, figsize = (15,10*rows_in_fig))
    
    for stock in stock_quote:
           
        if rows_in_fig == 1: # when there is only 1 stock
            
            # extract data from stock_quote
            stock_data = data.get_data_yahoo(stock, start = start_date, end = end_date)

            # plot to stock chart <- adj close at first
            ln1 = ax.plot(stock_data.index, stock_data['Adj Close'], color = 'red', label = stock_data['Adj Close'].name)

            # create a the second y-axis for trading volume
            axv = ax.twinx()

            # set up font size
            font = {'size': 16}

            # set up the x_label
            ax.set_xlabel(stock_data.index.name, fontdict = font)

            # set up the y label
            ax.set_ylabel(stock_data[['Adj Close']].columns[0], fontdict = {'size': 16})

            # set up the title
            ax.set_title(stock)

            # axv stands for ax volume, which is the other y-axis plotting on the same chart as stock price
            axv.set_ylabel(stock_data[['Volume']].columns[0], fontdict = {'size': 16})

            # only get the date_part from df.index
            ax.set_xticklabels(stock_data.index.date , rotation = 50)

            # show no grid
            ax.grid(False)

            # adjutst the volume scale as it overlaps with the stock price
            axv.set_ylim(0, stock_data['Volume'].max()*1.5)

            # fill between for axv
            ln2 = axv.fill_between(stock_data.index, stock_data['Volume'],step="pre", alpha = 0.2, color = 'purple', label = stock_data.Volume.name)

            # we can use list comprehension + regular expression
            # '{:,2f}'.format(y) to pre-process every y-ticks on the y-axis, converting trading volume the to the unit of millions
            yticks = ['{:,.0f}'.format(y) + ' ' +'m' for y in axv.get_yticks()/1000000]

            # assigned the processed yticks to the labels 
            axv.set_yticklabels(yticks)

            # get legend
            lines, labels = ax.get_legend_handles_labels()
            lines2, labels2 = axv.get_legend_handles_labels()
            ax.legend(lines + lines2, labels + labels2, loc='upper left', fontsize = 14)

            # plt.margins(float) <- axes margins betwee the plot and boundary of the figure
            plt.margins(0.02)

            # plt.tight_layout()
            plt.tight_layout(pad = 3)
            
            plt.style.use("ggplot")
            
        # once the the number of stock to plot is greater than the number of axes, the function will stop
        if rows_in_fig >1 and rows_in_fig  >= axes_counter:

            # extract data from stock_quote
            stock_data = data.get_data_yahoo(stock, start = start_date, end = end_date)

            # plot to stock chart <- adj close at first
            ln1 = ax[axes_counter - 1].plot(stock_data.index, stock_data['Adj Close'], color = 'red', label = stock_data['Adj Close'].name)

            # create a the second y-axis for trading volume
            axv = ax[axes_counter - 1].twinx()

            # set up font size
            font = {'size': 16}

            # set up the x_label
            ax[axes_counter - 1].set_xlabel(stock_data.index.name, fontdict = font)

            # set up the y label
            ax[axes_counter - 1].set_ylabel(stock_data[['Adj Close']].columns[0], fontdict = {'size': 16})

            # set up the title
            ax[axes_counter - 1].set_title(stock)

            # axv stands for ax volume, which is the other y-axis plotting on the same chart as stock price
            axv.set_ylabel(stock_data[['Volume']].columns[0], fontdict = {'size': 16})

            # only get the date_part from df.index
            ax[axes_counter - 1].set_xticklabels(stock_data.index.date , rotation = 50)

            # show no grid
            ax[axes_counter - 1].grid(False)

            # adjutst the volume scale as it overlaps with the stock price
            axv.set_ylim(0, stock_data['Volume'].max()*1.5)

            # fill between for axv
            ln2 = axv.fill_between(stock_data.index, stock_data['Volume'],step="pre", alpha = 0.2, color = 'purple', label = stock_data.Volume.name)

            # we can use list comprehension + regular expression
            # '{:,2f}'.format(y) to pre-process every y-ticks on the y-axis, converting trading volume the to the unit of millions
            yticks = ['{:,.0f}'.format(y) + ' ' +'m' for y in axv.get_yticks()/1000000]

            # assigned the processed yticks to the labels 
            axv.set_yticklabels(yticks)

            # get legend
            lines, labels = ax[axes_counter - 1].get_legend_handles_labels()
            lines2, labels2 = axv.get_legend_handles_labels()
            ax[axes_counter - 1].legend(lines + lines2, labels + labels2, loc='upper left', fontsize = 14)

            # plt.margins(float) <- axes margins betwee the plot and boundary of the figure
            plt.margins(0.02)

            # plt.tight_layout()
            plt.tight_layout(pad = 3)
            
            plt.style.use("ggplot")
            
            # next stock 
            axes_counter += 1

    plt.show()

plot_stock_volumne(['FB','AMZN','NFLX','GOOG'], '2017-01-01','2019-01-01')


```


## Here are the result after running the script

![Result](https://github.com/kwlwman2/Plot-the-Stock-Price-and-Trading-Volume/blob/master/result.png)


## Explanations and Limitations

Generally, whenever there is a big swing in stock price, the trading volume wil increase significantly. This phenomena makes sense because large trading volume will push the stock price either to rally or plumment. Trading volume is the sum of trading volume of sell side and buy side. Therefore, in order to better understand the market sentiment, it would better to segregate the sell-side volume and buy-side volume. Another improvement is to add annotation on the price chart to study what kind of event drives stock price to move drastically.
