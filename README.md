# Recession_Analysis
A recession is an economic situation that arrives when the circulation of money in the economy is low for two consecutive quarters. When the circulation of money is low, it means people are not spending money in the market. When people don’t spend money, businesses face losses, which results in an economic slowdown and layoffs, which you must have already heard about in 2023.

## Recession Analysis
Recession is calculated and analyzed according to the growth in GDP, the growth in the unemployment rate, and the growth in consumer spending rate. But the most common way of measuring recession is by analyzing the monthly GDP growth data.So, for the task of Recession analysis, we need to have a dataset of the monthly GDP growth of a country. I found an ideal dataset for this task that is based on the monthly GDP growth rate of the United Kingdom.

## Recession Analysis using Python
import pandas as pd  </p>
import plotly.graph_objs as go </p>
import plotly.express as px  </p>
import plotly.io as pio </p>

## Reading of Dataset

data = pd.read_csv('UK_monthly_gdp.csv')  </p>
print(data.head())   </p>

## Let’s have a look at the GDP growth over time: 

fig = go.Figure(data=go.Heatmap(   
                   z=[data['GDP Growth']],   
                   x=data.index,                   
                   y=['GDP Growth'],    
                   colorscale='Viridis'))   </p>

fig.update_layout(title='GDP Growth over Time',  
                  xaxis_title='Time Period',   
                  yaxis_title='')    

fig.show()   </p>

## As a recession means the decline in the circulation of money for two consecutive quarters, I will convert our monthly data into quarterly data to analyze the recession:  </p>

# Convert monthly data to quarterly data using resample method   </p>
data['Time Period'] = pd.to_datetime(data['Time Period'], format='/%m/%Y')  </p>
data.set_index('Time Period', inplace=True)  </p>
quarterly_data = data.resample('Q').mean()  </p>
print(quarterly_data.head())   </p>

## Now here’s how we can calculate and analyze recession based on quarterly GDP growth:  </p>

# Calculate recession based on quarterly GDP growth     </p>
quarterly_data['Recession'] = ((quarterly_data['GDP Growth'] < 0) & (quarterly_data['GDP Growth'].shift(1) < 0))   </p>

# Fill missing values with False (since the first quarter cannot be in a recession)   </p>
quarterly_data['Recession'].fillna(False, inplace=True)    </p>

# Plot the GDP growth and recession data   </p>
fig = go.Figure()     </p>
fig.add_trace(go.Scatter(x=quarterly_data.index,            
                         y=quarterly_data['GDP Growth'],           
                         name='GDP Growth',           
                         line=dict(color='green', width=2)))            
fig.add_trace(go.Scatter(x=quarterly_data[quarterly_data['Recession']].index,        
                         y=quarterly_data[quarterly_data['Recession']]['GDP Growth'],  
                         name='Recession', line=dict(color='red', width=2)))       

fig.update_layout(title='GDP Growth and Recession over Time (Quarterly Data)',            
                  xaxis_title='Time Period',                
                  yaxis_title='GDP Growth')         

fig.show() </p>

## Let us now analyze the severity of the recession. The severity of a recession refers to the extent to which the economy contracts during a recession. A severe recession involves a deeper and more prolonged decline in economic activity, resulting in negative effects on employment, incomes and other economic indicators. Here’s how to analyze the severity of the recession:       </p>
quarterly_data['Recession Start'] = quarterly_data['Recession'].ne(quarterly_data['Recession'].shift()).cumsum()      </p>
recession_periods = quarterly_data.groupby('Recession Start')         </p>
recession_duration = recession_periods.size()        </p>
recession_severity = recession_periods['GDP Growth'].sum()       </p>

fig = go.Figure()             </p>
fig.add_trace(go.Bar(x=recession_duration.index, y=recession_duration,           
                     name='Recession Duration'))                
fig.add_trace(go.Bar(x=recession_severity.index, y=recession_severity,      
                     name='Recession Severity'))         

fig.update_layout(title='Duration and Severity of Recession',         
                  xaxis_title='Recession Periods',              
                  yaxis_title='Duration/Severity')     

fig.show()           </p>
  
## Summary            </p>
A recession is an economic situation that arrives when the circulation of money in the economy is low for two consecutive quarters. Recession is calculated and analyzed according to the growth in GDP, the growth in the unemployment rate, and the growth in consumer spending rate. I hope you liked this article on Recession Analysis using Python. Feel free to ask valuable questions in the comments section below.
