---
layout: post
title: Loading and Exploring the TEP Dataset
date: 2019-12-05 12:43:03 +0100
categories: [Engineering, Anomaly Detection, TEP]
tags: [Engineering, Anomaly Detection, TEP]
seo:
  date_modified: 2020-01-06 10:20:35 +0200
---

In the [last blog post](https://keepfloyding.github.io/posts/Ten-East-Proc-Intro/), I explained why the Tennessee Eastman Process (TEP) dataset is commonly used as a benchmark for anomaly detection algorithms for chemical process data. I also mentioned that the open-source dataset can be found [here](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/6C3JR1) in the Harvard Dataverse archive. In this post, I'll perform some preliminary data exploration of the TEP data to show its main characteristics. 

# Loading the data

As mentioned before, the archive has a 'fault-free' and a 'faulty' dataset that corresponds to whether any faults have been introduced into the simulation model that generated the data. Let's start with the 'fault-free' training dataset. The files have a .RData extension and as such can be loaded into an R environment as follows:

{% highlight R %}
library(miceadds)
df <- load.Rdata2('TEP_FaultFree_Training.rdata', path=getwd())
write.csv(df, 'TEP_FaultFree_Training.csv')
{% endhighlight %} 

I typically prefer working in Python so I saved the file as a csv so that I can do some data wrangling with Pandas. In this dataset, there are a total of 55 columns comprised of the following features:

* xmeas_NUM(where NUM is a number from 1 to 41)
* xmv_NUM(where NUM is a number from 1 to 11)
* simulationRun
* sample
* faultNumber

The variables starting with xmeas correspond to 41 measured variables of the TEP process while the variables starting with xmv correspond to 11 manipulated variables (i.e. parameters that can be controlled by the person operating the process). The values of these variables are determined by a computational simulation of the TEP that runs for 25 hours (for training datasets, 48 hours for the testing dataset). This simulation is run for 500 times each with a different, but non-overlapping number generator state as indicated by the 'simulationRun' column. 

# What does sample, simulationRun and faultNumber mean?

'Sample' in this case refers to each sampled timestep of the 'simulationRun', which for this dataset goes from 1 to 500. Each sample is a 3 minute interval, which multiplied by 500 samples passes the sanity check of the 25 hours stated earlier. Lastly, 'faultNumber' corresponds to which fault was given to each simulation. Since this is the 'fault-free' dataset, this column has a value of 0 for all rows indicating that there is normal operation throughout. I've put some sample python code here for loading and taking a preliminary look at the dataset:

{% highlight python %}
# Loading all the required packages
import pandas as pd
import matplotlib.pyplot as plt

# Loading data
df = pd.read_csv('TEP_FaultFree_Training.csv', index_col=0)
df.head()

# Fault number should be 0 throughout
df['faultNumber'].value_counts()

# Checking that each simulation run has 500 entries
df['simulationRun'].value_counts().sort_values(ascending=False).plot(style='o')
print(df['simulationRun'].max())

# Checking that each sample also has 500 entries (only for training case, test should have 960)
df['sample'].value_counts().sort_values(ascending=False).plot(style='o')
print(df['sample'].max())

# Sanity check, number of total entries should be max. simulationNum times max. sample
assert len(df)==df['simulationRun'].max()*df['sample'].max(), 'sanity check does not hold'
{% endhighlight %} 


# Renaming XMEAS and XMV variables

Now the first problem that we run into is identifying what each xmeas and each xmv refers to (since they should represent actual physical values). Luckily, [this source](https://www.sciencedirect.com/science/article/pii/S0098135414000969?via%3Dihub) provides what each xmeas and xmv variable refers to. The variables can hence be renamed as follows:

{% highlight python %}
X_dict = {
'XMEAS_1':'A_feed_stream',
'XMEAS_2':'D_feed_stream',
'XMEAS_3':'E_feed_stream',
'XMEAS_4':'Total_fresh_feed_stripper',
'XMEAS_5':'Recycle_flow_into_rxtr',
'XMEAS_6':'Reactor_feed_rate',
'XMEAS_7':'Reactor_pressure',
'XMEAS_8':'Reactor_level',
'XMEAS_9':'Reactor_temp',
'XMEAS_10':'Purge_rate',
'XMEAS_11':'Separator_temp',
'XMEAS_12':'Separator_level',
'XMEAS_13':'Separator_pressure',
'XMEAS_14':'Separator_underflow',
'XMEAS_15':'Stripper_level',
'XMEAS_16':'Stripper_pressure',
'XMEAS_17':'Stripper_underflow',
'XMEAS_18':'Stripper_temperature',
'XMEAS_19':'Stripper_steam_flow',
'XMEAS_20':'Compressor_work',
'XMEAS_21':'Reactor_cooling_water_outlet_temp',
'XMEAS_22':'Condenser_cooling_water_outlet_temp',
'XMEAS_23':'Composition_of_A_rxtr_feed',
'XMEAS_24':'Composition_of_B_rxtr_feed',
'XMEAS_25':'Composition_of_C_rxtr_feed',
'XMEAS_26':'Composition_of_D_rxtr_feed',
'XMEAS_27':'Composition_of_E_rxtr_feed',
'XMEAS_28':'Composition_of_F_rxtr_feed',
'XMEAS_29':'Composition_of_A_purge',
'XMEAS_30':'Composition_of_B_purge',
'XMEAS_31':'Composition_of_C_purge',
'XMEAS_32':'Composition_of_D_purge',
'XMEAS_33':'Composition_of_E_purge',
'XMEAS_34':'Composition_of_F_purge',
'XMEAS_35':'Composition_of_G_purge',
'XMEAS_36':'Composition_of_H_purge',`
'XMEAS_37':'Composition_of_D_product',
'XMEAS_38':'Composition_of_E_product',
'XMEAS_39':'Composition_of_F_product',
'XMEAS_40':'Composition_of_G_product',
'XMEAS_41':'Composition_of_H_product',
'XMV_1':'D_feed_flow_valve',
'XMV_2':'E_feed_flow_valve',
'XMV_3':'A_feed_flow_valve',
'XMV_4':'Total_feed_flow_stripper_valve',
'XMV_5':'Compressor_recycle_valve',
'XMV_6':'Purge_valve',
'XMV_7':'Separator_pot_liquid_flow_valve',
'XMV_8':'Stripper_liquid_product_flow_valve',
'XMV_9':'Stripper_steam_valve',
'XMV_10':'Reactor_cooling_water_flow_valve',
'XMV_11':'Condenser_cooling_water_flow_valve',
'XMV_12':'Agitator_speed'
   }

df = df.rename(columns = lambda x:X_dict[x.upper()] if x.upper() in X_dict.keys()  else x)
{% endhighlight %} 

Note that XMV_12 is missing (possibly due to the difficulties in accurately determining the effect that agitator speed has on the overall reaction efficiency). The same method that is illustrated here for reading and renaming the data should apply to the other TEP datasets on the Harvard Dataverse platform. In the next blog post, we will begin to explore how these measured variables change over time for each simulation. Hope to see you next time!








