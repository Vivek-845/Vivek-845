//Average latency from data source
 
defineTable(query={
  readFile(["TestVivek.csv"])
}, include=[*], name="TestVivek", view="search-all")
| match(table="TestVivek", field=[#event.dataset], column=[#event.dataset], include=[#Vendor, Priority], strict=true)
| eval(latency=(@ingesttimestamp-@timestamp)/1000)
//| groupBy(#event.dataset, #Vendor, function=avg(latency, as="latency (in sec)"))
| groupBy(#event.dataset, #Vendor, function=(avg(latency)))
| case {
  _avg < 60 | _avg := math:floor("_avg") | format("%s sec", field=["_avg"], as = latency) ;
  _avg >= 60 and _avg < 3600000 | _avg := _avg / 60 | _avg := math:floor("_avg") | format("%s min", field=["_avg"], as = latency) ;
  _avg >= 3600000 and _avg < 86400000 | _avg := _avg / 3600 | _avg := math:floor("_avg") | format("%s hr", field=["_avg"], as = latency) ;
 
}
| select(#event.dataset, #Vendor, latency)
| sort(latency, order=desc)
