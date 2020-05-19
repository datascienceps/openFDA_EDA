# openfda_exploration
Exploring adverse drug reactions with reports from the OpenFDA API

As part of a toy challenge, I explored the FDA's database of adverse event reports via the OpenFDA web API: https://open.fda.gov/drug/event/. These reports contains general information about the report, patient information, a list of the drugs that the patient is taking, and a list of patient reactions. In this brief analyis, I explore this question (in addition to a few smaller sub-questions):  
 
 
•	Are different adverse events reported in different countries?  


The code is in a Jupyter Notebook and includes both the API queries for downloading data, and for analysis/visualisation. and I have also included a few smaller external datasets that I used to analyse the data. 

Analysis
-

Please download the notebook and run the cells!

Looking into the data ourselves, a few things are immediately obvious.  

1. The US is really over represented in the data!  

![picture](data/events_by_country.png)

This makes sense since the FDA primarily acts within the United States and associated territories. However, there are a significant numbers of adverse events reported from Europe, Asia and South America. This may be interesting since - to my current understanding - drugs approved by the FDA are approved for use within the US. Perhaps these reported events are from travellers, military, or other US citizens.  


2. The top 4 most reported adverse events:  
  - OFF LABEL USE 
  - PRODUCT USE ISSUE  
  - INAPPROPRIATE SCHEDULE OF DRUG ADMINISTRATION  
  - NO ADVERSE EVENT  

![alt text](data/common_events.png)

This is interesting since OFF LABEL USE - the use of pharmaceutical drugs for an unapproved indication or in an unapproved age group, dosage, or route of administration - might account for those overseas reported events! It also represents an issue for pharmaceutical companies, since their medicines are being in inapproroprate and potentially dangerous ways. Further analysing this specific event, we also see that it is mainly "CONSUMERS / NON-MEDICAL PROFESSIONALS" who report OFF LABEL USE; we might expect these kinds of events to reported by medical professionals! The 'NO ADVERSE EVENT' is also not quite intuitive - why was an adverse report submitted to say there was no adverse event? There is probably reason which should present itself with some more digging into the API documentation!

The next 21 most common reported events are much more biologically-related:  

       'DRUG+DOSE+OMISSION', 'LOSS+OF+CONSCIOUSNESS', 'PAIN+IN+EXTREMITY',
       'INJECTION+SITE+PAIN', 'ABDOMINAL+PAIN+UPPER', 'ABDOMINAL+PAIN',
       'CHEST+PAIN', 'BACK+PAIN', 'DRUG+HYPERSENSITIVITY', 'WEIGHT+DECREASED',
       'DRUG+INEFFECTIVE', 'DRUG+INTERACTION', 'HAEMOGLOBIN+DECREASED',
       'BLOOD+PRESSURE+INCREASED', 'BLOOD+GLUCOSE+INCREASED',
       'WEIGHT+INCREASED', 'HEART+RATE+INCREASED',
       'INCORRECT+DOSE+ADMINISTERED', 'PRODUCT+QUALITY+ISSUE', 'PAIN',
       'CARDIAC+DISORDER']


Building upon the initial analysis
-

The dataset is large and very rich! Lots of questions can be asked:  

•	Are different adverse events associated with different disease areas?
•	What drugs tend to be taken together?  
•	How do other factors (patient age, location, route of administration) affect adverse events?
•	Do any drug combinations tend to associate with certain adverse events?

A few other ways we might extend the analysis:

1. One interesting idea here is to download the entire dataset from OpenFDA (around 100GB), and store it in a graph database. These are particularly useful for representing complex relationship, but also mining for new insights and correlations! The current API is very robust, and these questions can be answered using well-constructed API queries.

2. Time-series: my analysis relied largely on 'counts', or raw counts given certain conditions; i.e.:

'https://api.fda.gov/drug/event.json?search=patient.reaction.reactionmeddrapt.exact:OFF+LABEL+USE&count=primarysource.qualification

#Get counts of the different qualifications of the primary sources (physician, consumer, etc...) of each adverse report related to 'OFF LABEL USE''

However, as far as I understand, these counts do not take into account the time-series aspect of the dataset. The dataset extends as far back as 2004; so there is definitely the possibility to observe changes in adverse events/drug usage/etc over time.

3. External datasets: I augmented my analysis in a very small way by grouping the 178 countries in the dataset into continents; using official classifications and the 2-digit ISO code standard utilised by OpenFDA. However, the analysis can be supported in interesting ways by integrating other datasets such as GDP, education attainment, poverty, hygiene. These data are readily available online from reliable sources such as the World Health Organisation and the World Bank.

4. Thinking about dervining actionable or commercial insights, the OpenFDA API clearly states that "not all data in openFDA has been validated for clinical or production use." However, there are still definitely interesting insights and trends to be explored - and which could lead to further projects, possibly integrating external datasets or validated FDA data. Additionally, further insights could be mined by integrating this data with similar datasets from other regulatory agencies such as the European Medicines Agency (EMA).  


Limitations of the data
---------

There are a number of limitations relating to the data itself; as discussed by the FDA:

https://open.fda.gov/update/ten-things-to-know-about-adverse-events/

"The reports within the dataset do not undergo extensive screening or validation. There is no certainty that the reported event (adverse event or medication error) was actually due to the product. FDA does not require that a causal relationship between a product and event be proven, and reports do not always contain enough detail to properly evaluate an event.

As we rely on voluntary reports for adverse events, FDA does not receive reports for every adverse event or medication error that occurs with a product. Many factors can influence whether or not an event will be reported, such as the time a product has been marketed and publicity about an event. Therefore, the data cannot be used to calculate the incidence of an adverse event or medication error in the U.S. population."

Given these limits of the dataset, it's probably best to derive aggregate statistics on the data to gain 'insights on trends' in the data, as opposed to clinically actionable facts or evidence regarding specific adverse events. 

CONCLUSION
-

This is a really rich dataset and I hope this very small contribution has given you a flavour of what can be done with the data. Please feel to reach out if I can help you conduct your own analyses in any way!

