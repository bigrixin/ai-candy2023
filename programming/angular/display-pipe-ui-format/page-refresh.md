# Page Refresh



* **refresh a whole page**\
  &#x20; window.location.reload();\

*   **delay refreshing a whole page**\


    //refresh page delay 3 seconds setTimeout(window.location.reload.bind(location),3000);\


    //same above \
    setTimeout(location.reload.bind(location), 3000);\

*   **reload partial page**\


    this.reload(this.router.url);\
    \
    // reload function

    async reload(url: string): Promise { \
    &#x20;      await this.router.navigateByUrl('otherpage', { skipLocationChange: true });     \
    &#x20;      return this.router.navigateByUrl(url); \
    }\

* **delay reload partial page**\
  \
  this.delayReload(3000); //1000ms \
  \
  // delay reload function\
  async delayReload(ms: any) { \
  &#x20;         this.router.navigate(\[this.router.url]) .then(() => { \
  &#x20;              setTimeout(() => { \
  &#x20;                   // window.location.reload(); \
  &#x20;                   this.router.navigate(\['/dashboard']); \
  &#x20;               }, ms); \
  &#x20;          });\
  &#x20;}

