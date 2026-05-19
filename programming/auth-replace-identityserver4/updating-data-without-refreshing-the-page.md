# Updating data without refreshing the page

{% code fullWidth="true" %}
```typescript
  data: any;
  private refreshInterval = 60 * 1000; // ms
  private intervalSubscription!: Subscription;

  constructor(
    private dashboardDataService: DashboardDataService,
  ) { }

  ngOnInit(): void {
    this.loadData();
    this.intervalSubscription = interval(this.refreshInterval).subscribe(() => {
      this.loadData();
    });
  }

  ngOnDestroy(): void {
    if (this.intervalSubscription) {
      this.intervalSubscription.unsubscribe();
    }
  }

  private loadData(): void {
    this.dashboardDataService.getData().subscribe({
      next: (res: any) => {
        this.data = res;
        ....
        ....
        ....
      },
      error: (error: any) => {
        console.error('Error fetching data', error);
      }
    });
  }
```
{% endcode %}
