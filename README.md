// Total data sources

| groupBy(#event.dataset)
| count()

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

// Top 10 data sources

defineTable(query={
  readFile(["TestVivek.csv"])
}, include=[*], name="TestVivek", view="search-all")
| match(table="TestVivek", field=[#event.dataset], column=[#event.dataset], include=[#Vendor, Priority], strict=true)
| #event.dataset=?event.dataset Priority=?Priority #event.module=?event.module #Vendor=?Vendor
| top([#event.dataset, #event.module, #Vendor, Priority], limit=10)
| select(#event.dataset, #event.module, #Vendor, Priority)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

// Log ingestion status per data source in last 2 hrs

defineTable(query={
  readFile(["TestVivek.csv"])
}, include=[*], name="TestVivek", view="search-all")
| match(table="TestVivek", field=[#event.dataset], column=[#event.dataset], include=[Priority], strict=true)
| #event.dataset=?event.dataset Priority=?Priority
| groupBy(#event.dataset, function=selectLast(@ingesttimestamp, #event.module, #Vendor, Priority), limit=max)
| age_mins := (now() - @ingesttimestamp) / 60000
| case {
  age_mins > 120 | status := "No logs in in last 2 Hr";
  age_mins < 120 | status := "Logs received in last 2 Hr";
}
| select(#event.dataset,  #event.module, #Vendor, status, Priority)
| #event.module=?event.module #Vendor=?Vendor

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

// Log ingestion status per data source in last 1 Hr

defineTable(query={
  readFile(["TestVivek.csv"])
}, include=[*], name="TestVivek", view="search-all")
| match(table="TestVivek", field=[#event.dataset], column=[#event.dataset], include=[Priority], strict=true)
| #event.dataset=?event.dataset Priority=?Priority
| groupBy(#event.dataset, function=selectLast(@ingesttimestamp, #event.module, #Vendor, Priority), limit=max)
| age_mins := (now() - @ingesttimestamp) / 60000
| case {
  age_mins > 60 | status := "No logs in in last 1 Hr";
  age_mins < 60 | status := "Logs received in last 1 Hr";
}
| select(#event.dataset,  #event.module, #Vendor, status, Priority)
| #event.module=?event.module #Vendor=?Vendor
