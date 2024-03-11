# Partial columns update

{% code fullWidth="true" %}
```
// Edit partial columns

1. Auto Mapping:

  CreateMap<HouseViewModel, House>()
    .ForAllMembers(opts => opts.Condition((src, dest, srcMember) => srcMember != null));


2. Model

    public double? Price { get; set; }         // int/double/boolen type need a ?
    public DateTime? Timestamp { get; set; }   // DateTime type need a ?
    public string Status { get; set; }         // only string type do not need a ? 
    public bool Sold { get; set; }             // must get value before mapping

3. Add old value in ViewModel

   var formData = this.buildFormData(formGroupData);
   buildFormData(formGroupData: any) {
    var formData: any = new FormData();
    formData.append("id",formGroupData['id']);
    formData.append("Price", formGroupData['price'] != null ? formGroupData['price'] : "");
    formData.append("Sold", formGroupData['sold']);  // must be
    formData.append("TransactionNo", formGroupData['transactionNo']);

    return formData;
   }

    return this.httpClient.put<HttpResponse<Object>>(endPoint, formData, { observe: 'response' })
      .pipe(resp => resp);


```
{% endcode %}
