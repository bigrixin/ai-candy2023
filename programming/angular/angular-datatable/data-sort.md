# Data sort



{% code fullWidth="true" %}
```
// alphabetical order

 list = [
    {
      "id": 1,
      "name": "location 1",
    },
    {
      "id": 2,
      "name": "location 3",
    },
    {
      "id": 3,
      "name": "location 2",
    },


typeList = list.sort((a: { name: string; },b: { name: any; })=>a.name.localeCompare(b.name))



public sortLocationsAsc() {
  this.locations = this.locations.sort((a, b) => b.price - a.price);
}

public sortLocationsDesc(): void {
  this.locations = this.locations.sort((a, b) => a.price - b.price);
}

//---------------------------------

this.dtOptions = {
  scrollY: 720,
  scrollCollapse: true,
  autoWidth: true,
  pagingType: 'full_numbers',
  pageLength: 15,
  lengthMenu: [10, 15, 20, 50, 100],
  order:[[1,'asc']]   ///asc or desc
};

```
{% endcode %}
