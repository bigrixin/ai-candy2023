# Datatables save state using localStorage

{% code fullWidth="true" %}
```typescript
this.dtOptions = {
  。。。
  
  stateSave: true,  // Save the state of the table(pagination, sorting, filtering, etc.) 
  stateSaveCallback: function (settings: any, data: any) {
     localStorage.setItem('DataTables_weighing', JSON.stringify(data));
  },
  stateLoadCallback: function (settings: any) {
    let savedState =  localStorage.getItem('DataTables_weighing') || '{}';
    // If no saved state, return empty state to prevent errors
    if (savedState) {
      try {
        return JSON.parse(savedState);
      } catch (e) {
        console.error('Error loading saved DataTable state:', e);
        return {}; // Return an empty state if parsing fails
      }
    } else {
      return {}; // If no saved state, return empty object
  }
  },
  stateDuration: 0, // 1:no time limit for localStorage,  (1day）= 60 * 60 * 24, -1:for sessionStorage,

}
```
{% endcode %}
