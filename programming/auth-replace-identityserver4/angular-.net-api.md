# Angular - .NET API

{% code fullWidth="true" %}
```
// post, put and delete


addNewVehicle(formGroupData: any): Observable<any> {
   var formData = this.buildFormData(formGroupData);
   let endPoint = this.rootURL + 'api/vehicle/create';
  ///var jsonBody = JSON.stringify(formGroupData);   //to JSON
  return this.httpClient.post<HttpResponse<Object>>(endPoint, formData, { observe: 'response' }).pipe(resp => resp);
};


editVehicle(formGroupData: any): Observable<any> {

  let jsonOption = { 'content-type': 'application/json', 'charset': 'utf-8' };
  let header = new HttpHeaders(jsonOption);  
  var id = formGroupData['vehicleid'];
  let endPoint = this.rootURL + 'api/vehicle/edit/' + String(id);
  var jsonBody = JSON.stringify(formGroupData);   //to JSON -- use FromBody on .NET API
  return this.httpClient.put<HttpResponse<Object>>(endPoint, jsonBody, { observe: 'response', headers:  header }).pipe(resp => resp);
};


deleteVehicle(id: number): Observable<any> {
  let endPoint = this.rootURL + 'api/vehicle/delete/' + id;
  return this.httpClient.delete<HttpResponse<Object>>(endPoint, { observe: 'response' })
    .pipe(resp => resp);
}


//#region functions

buildFormData(formGroupData: any) {
  var formData: any = new FormData();
  formData.append("Rego", formGroupData['rego'] != null ? formGroupData['rego']:"''");
  formData.append("Tare", formGroupData['tare']!= null ? formGroupData['tare'] : 0);
  formData.append("Customerid", formGroupData['customerid']);
  return formData;
}

//#region
```
{% endcode %}

{% code fullWidth="true" %}
```
// .NET [FromForm] and [FromBody]
//  POST, PUT AND DELETE

[HttpPost]
[Route("api/vehicle/create")]
public async Task<IActionResult> CreateVehicle([FromForm] VehicleViewModel vehicleVM)
{
    if (!ModelState.IsValid)
        return BadRequest(ModelState);
    ......


[HttpPut]
[Route("api/vehicle/edit/{id}")]
public async Task<IActionResult> UpdateVehicle(long id, [FromBody] VehicleViewModel vehicleVM)
{
    if (!ModelState.IsValid)
        return BadRequest(ModelState);

    .....


[HttpDelete]
[Route("api/vehicle/delete/{id}")]
public async Task<IActionResult> Delete(long id)

```
{% endcode %}
