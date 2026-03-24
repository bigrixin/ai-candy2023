# 🌛 Solar date to BaZi

{% code fullWidth="true" %}
```
// Some code

  private readonly TIAN_GAN = ['甲', '乙', '丙', '丁', '戊', '己', '庚', '辛', '壬', '癸'];
  private readonly DI_ZHI = ['子', '丑', '寅', '卯', '辰', '巳', '午', '未', '申', '酉', '戌', '亥'];

 // 年干支
  private calcYearGZ(year: number): string {
    const baseYear = 1984;
    const offset = year - baseYear;
    const ganIndex = (offset % 10 + 10) % 10;
    const zhiIndex = (offset % 12 + 12) % 12;
    return this.TIAN_GAN[ganIndex] + this.DI_ZHI[zhiIndex];
  }

  // 月干支（节气+五虎遁）
  private calcMonthGZ(year: number, month: number, day: number): string {
    const zhi = this.getMonthZhiByJieQi(month, day);
    const yearGan = this.calcYearGZ(year)[0];
    let firstMonthGan: string;
    if (yearGan === '甲' || yearGan === '己') firstMonthGan = '丙';
    else if (yearGan === '乙' || yearGan === '庚') firstMonthGan = '戊';
    else if (yearGan === '丙' || yearGan === '辛') firstMonthGan = '庚';
    else if (yearGan === '丁' || yearGan === '壬') firstMonthGan = '壬';
    else firstMonthGan = '甲';
    const startIndex = this.TIAN_GAN.indexOf(firstMonthGan);
    const yueIndex = this.DI_ZHI.indexOf(zhi) - this.DI_ZHI.indexOf('寅');
    const ganIndex = (startIndex + yueIndex + 10) % 10;
    return this.TIAN_GAN[ganIndex] + zhi;
  }

  private getMonthZhiByJieQi(month: number, day: number): string {
    if ((month === 2 && day >= 4) || (month === 3 && day < 6)) return '寅';
    if ((month === 3 && day >= 6) || (month === 4 && day < 5)) return '卯';
    if ((month === 4 && day >= 5) || (month === 5 && day < 6)) return '辰';
    if ((month === 5 && day >= 6) || (month === 6 && day < 6)) return '巳';
    if ((month === 6 && day >= 6) || (month === 7 && day < 7)) return '午';
    if ((month === 7 && day >= 7) || (month === 8 && day < 8)) return '未';
    if ((month === 8 && day >= 8) || (month === 9 && day < 8)) return '申';
    if ((month === 9 && day >= 8) || (month === 10 && day < 8)) return '酉';
    if ((month === 10 && day >= 8) || (month === 11 && day < 7)) return '戌';
    if ((month === 11 && day >= 7) || (month === 12 && day < 7)) return '亥';
    if ((month === 12 && day >= 7) || (month === 1 && day < 5)) return '子';
    return '丑';
  }

  // ==========================
  // ✅ 日干支：使用本地时间（北京时间）计算
  // 基准：1900-01-01 = 癸巳日（位置10）
  // ==========================
  private calcDayGanZhi(y: number, m: number, d: number): string {
    const MS_PER_DAY = 86400000;
    const TZ_OFFSET = 8 * 60 * 60 * 1000; // 北京时间

    const baseUTC = Date.UTC(1900, 0, 1);
    const targetUTC = Date.UTC(y, m - 1, d);

    const days = Math.floor((targetUTC + TZ_OFFSET - baseUTC - TZ_OFFSET) / MS_PER_DAY);

    const idx = ((10 + days) % 60 + 60) % 60;

    const gan = this.TIAN_GAN[idx % 10];
    const zhi = this.DI_ZHI[idx % 12];
    return gan + zhi;
  }


  // ==========================
  // ✅ 时柱五鼠遁 
  // ==========================
  private calcHourGanZhi(dayGan: string, hour: number): string {
    const h = Math.floor((hour + 1) / 2) % 12;

    const dayGanIdx = this.TIAN_GAN.indexOf(dayGan.charAt(0));

    // 五鼠遁起点
    const startMap = [0, 2, 4, 6, 8, 0, 2, 4, 6, 8];

    const ganIdx = (startMap[dayGanIdx] + h) % 10;

    return this.TIAN_GAN[ganIdx] + this.DI_ZHI[h];
  }

```
{% endcode %}
