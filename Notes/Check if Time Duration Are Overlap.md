---
category: note
type: tip
tags:
  - js
  - ts
  - dayjs
  - date
  - one_heart
description: 確認時間區間是否有部分重疊
---
有時候我們會需要確認兩個時間區間是否有部分重疊，例如：
- 時間A：10:00 ~ 12:10
- 時間B：12:00 ~ 13:00
此時的 A 跟 B 重疊，又例如：
- 時間A：10:00 ~ 14:00
- 時間B：12:00 ~ 13:00
此時的 A 跟 B 也是重疊，又例如：
- 時間A：10:00 ~ 12:00
- 時間B：12:00 ~ 13:00
此時則沒有重疊

由於我們比較的是時間區間所以結束時間一定會晚約或等於開始時間，因此我們只需要考慮以下情況便可以將非重疊的集合抓出來：
- A 的 **結束時間** 是否 `早於或等於` B 的 **開始時間**
- B 的 **結束時間** 是否 `早於或等於` A 的 **開始時間**
剩下的集合就會是重疊，因此我們可以用以下方式寫出 util：
```ts
function checkTimeOverlap(time1Start: string, time1End: string, time2Start: string, time2End: string) {
    const datePrefix = '2000/01/01 '; // 確保只比較時間

    const ts1_start = new Date(datePrefix + time1Start).getTime();
    const ts1_end = new Date(datePrefix + time1End).getTime();
    const ts2_start = new Date(datePrefix + time2Start).getTime();
    const ts2_end = new Date(datePrefix + time2End).getTime();

    // 如果時間段1的結束點在時間段2的開始點之前或相同
    // 或者時間段2的結束點在時間段1的開始點之前或相同
    // 則表示沒有重疊
    if (ts1_end <= ts2_start || ts2_end <= ts1_start) {
        return false;
    }

    // 否則，表示有重疊
    return true;
}
```

也可以使用 dayjs：
```ts
import dayjs from 'dayjs';
// import isSameOrBefore from 'dayjs/plugin/isSameOrBefore';
// dayjs.extend(isSameOrBefore); // 如果沒有使用 isBetween，這行可以移除

function checkTimeOverlapDayjs(time1Start: string, time1End: string, time2Start: string, time2End: string) {
    const datePrefix = '2000-01-01T'; 
    
    const d1_start = dayjs(datePrefix + time1Start);
    const d1_end = dayjs(datePrefix + time1End);
    const d2_start = dayjs(datePrefix + time2Start);
    const d2_end = dayjs(datePrefix + time2End);

    // 如果 d1_end 在 d2_start 之前或相同
    // 或者 d2_end 在 d1_start 之前或相同
    // 則表示沒有重疊
    if (d1_end.isSameOrBefore(d2_start) || d2_end.isSameOrBefore(d1_start)) {
        return false;
    }

    // 否則，表示有重疊
    return true;
}
```