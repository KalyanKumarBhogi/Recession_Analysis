# Recession_Analysis
A recession is an economic situation that arrives when the circulation of money in the economy is low for two consecutive quarters. When the circulation of money is low, it means people are not spending money in the market. When people don’t spend money, businesses face losses, which results in an economic slowdown and layoffs, which you must have already heard about in 2023.

# Recession Analysis
**Recession is calculated and analyzed according to the growth in GDP, the growth in the unemployment rate, and the growth in consumer spending rate. But the most common way of measuring recession is by analyzing the monthly GDP growth data.So, for the task of Recession analysis, we need to have a dataset of the monthly GDP growth of a country. I found an ideal dataset for this task that is based on the monthly GDP growth rate of the United Kingdom.**

**Recession Analysis using Python**
import pandas as pd  </p>
import plotly.graph_objs as go </p>
import plotly.express as px  </p>
import plotly.io as pio </p>

**Reading of Dataset**

data = pd.read_csv('UK_monthly_gdp.csv')  </p>
print(data.head())   </p>

![image](https://github.com/KalyanKumarBhogi/Recession_Analysis/assets/144279085/1c197f24-8de0-4599-aa6d-67af197abe5a)

**Let’s have a look at the GDP growth over time:** 

fig = go.Figure(data=go.Heatmap(   
                   z=[data['GDP Growth']],   
                   x=data.index,                   
                   y=['GDP Growth'],    
                   colorscale='Viridis'))   </p>

fig.update_layout(title='GDP Growth over Time',  
                  xaxis_title='Time Period',   
                  yaxis_title='')    

fig.show()   </p>

![image](https://github.com/KalyanKumarBhogi/Recession_Analysis/assets/144279085/124001f8-5775-449a-85e9-f925645a9a79)

**As a recession means the decline in the circulation of money for two consecutive quarters, I will convert our monthly data into quarterly data to analyze the recession:**  </p>

**Convert monthly data to quarterly data using resample method**   </p>
data['Time Period'] = pd.to_datetime(data['Time Period'], format='/%m/%Y')  </p>
data.set_index('Time Period', inplace=True)  </p>
quarterly_data = data.resample('Q').mean()  </p>
print(quarterly_data.head())   </p>
![image](https://github.com/KalyanKumarBhogi/Recession_Analysis/assets/144279085/267c0c0b-3571-4b43-b71a-8a1985752d7f)

**Now here’s how we can calculate and analyze recession based on quarterly GDP growth:**  </p>

**Calculate recession based on quarterly GDP growth**     </p>
quarterly_data['Recession'] = ((quarterly_data['GDP Growth'] < 0) & (quarterly_data['GDP Growth'].shift(1) < 0))   </p>

**Fill missing values with False (since the first quarter cannot be in a recession)**   </p>
quarterly_data['Recession'].fillna(False, inplace=True)    </p>

**Plot the GDP growth and recession data**   </p>
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

![image](https://github.com/KalyanKumarBhogi/Recession_Analysis/assets/144279085/b6691403-68c0-4896-84a4-6a09d003491f)


**Let us now analyze the severity of the recession. The severity of a recession refers to the extent to which the economy contracts during a recession. A severe recession involves a deeper and more prolonged decline in economic activity, resulting in negative effects on employment, incomes and other economic indicators. Here’s how to analyze the severity of the recession:** </p>
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

![image](https://github.com/KalyanKumarBhogi/Recession_Analysis/assets/144279085/3168f73d-50b7-45e2-8cca-5e309c8fed08)

# Conclusion    </p>
**The recession analysis of the United Kingdom's monthly GDP growth data provides valuable insights into economic trends and downturns. The heatmap visualization illustrates the fluctuation in GDP growth over time, laying the foundation for further analysis. The conversion of monthly data into quarterly data allows for a more comprehensive assessment of recessions based on consecutive quarters of negative GDP growth. The plotted GDP growth and recession data showcase periods of economic contractions, with red lines indicating recession quarters. Further exploration into the severity of recessions reveals both the duration and severity of each recession period. This analysis equips policymakers and economists with a comprehensive understanding of the economic landscape, enabling them to make informed decisions to mitigate the impact of recessions and foster economic resilience.**
