---
description: column definition
---

# Angular DataTable

{% code fullWidth="true" %}
```
// HTML or ts column definition

            next: resp => {
              // if load data by html, need to remark
             ///////  this.WeighingList = resp.data;   // this line is for html to load data
     

              callback({
                recordsTotal: resp.recordsTotal,
                recordsFiltered: resp.recordsFiltered,
               /////  "data: []"    // this line is for html to load data
                data: resp.data  // set data, this line is for .ts to load data, it need below columns:[....]
              });

.....

      columns: [
        { data: 'ticketno' },
        {
          data: 'product', render: function (data: any, type: any, row: any) {
            if (data.substring(0, 6) == 'AAAA')
              return '<span style="color:RoyalBlue">' + row.product + '</span> <a class="pointer text-success" style="cursor:pointer;(click)="barleyInfo(\'' + row.location + '\',\'' + row.transactionno + '\')" > <i class="bi bi-info-circle"></i></a>'
            else if (data.substring(0, 5) == 'BBBB')
              return '<span style="color:RoyalBlue">' + row.product + '</span> <a class="pointer text-success" style="cursor:pointer;(click)="wheatInfo(\'' + row.location + '\',\'' + row.transactionno + '\')" > <i class="bi bi-info-circle"></i></a>'
            else if (data.substring(0, 6) == 'CCCC')
              return '<span style="color:RoyalBlue">' + row.product + '</span> <a class="pointer text-success" style="cursor:pointer;(click)="canolaInfo(\'' + row.location + '\',\'' + row.transactionno + '\')" > <i class="bi bi-info-circle"></i></a>'
            else
              return row.product
          }
        },
        { data: 'direction', render: function (data: any, type: any, row: any) {
           if (data =='IN')
             return '<span style="color:red;">' + row.direction + '</span>'
           else
             return '<span style="color:green;">' + row.direction + '</span>'
        } },
        { data: 'weight' , render: function (data: any, type: any, row: any) { return '<span style="color:rgb(145, 72, 39);">' + row.weight + '</span>' } },
        { data: 'timestamp' , render: function (data: any, type: any, row: any) { return '<span style="color:rgb(145, 72, 39);">' + row.timestamp + '</span>' } },
      ],
      rowCallback: (row: Node, data: any[] | Object, index: number) => {
        const self = this;
        $('a', row).off('click');
        $('a', row).on('click', () => {
          self.someClickHandler(data);
        });
        return row;
      }
    };

  }

  ngAfterViewInit() {
    setTimeout(() => {
      $($.fn.dataTable.tables(true)).DataTable().columns.adjust();
    }, 200);

    this.renderer.listen('document', 'click', (event) => {
      if (event.target.hasAttribute("view-person-id")) {
        console.log('Clicking the button: ', event);
      }
    });

  }

  someClickHandler(data: any): void {
    if (data.product.substring(0, 6) == "AAAA") {
      this.barleyInfo(data.location, data.transactionno);
    }
    else if (data.product.substring(0, 5) == "BBBB") {
      this.wheatInfo(data.location, data.transactionno);
    }
    else if (data.product.substring(0, 6) == "CCCC") {
      this.canolaInfo(data.location, data.transactionno);
    }
  }

```
{% endcode %}
