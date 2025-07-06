---
category: note
type: problem
tags:
  - vue
  - router
description: 因為 Parent Route Hook 觸發導致頁面無限迴圈
resource_1: "[[Vue Router Hook Causing Infinite Page Loop]]"
---
## Background

我正在做一個薪資詳情的功能，觀看這個薪資詳情頁需要先驗證密碼，因此使用者會先被導到驗證密碼頁，當其驗證成功後才會被導到薪資詳情的頁面，而薪資詳情頁面又根據使用者的角色區分為好幾個不同的子頁面，因此我便在這個薪資詳情（parent）新增了 beforeEnter hook 來檢查使用者是否已經驗證，並導到其對應的頁面，以下為薪資詳情的 route 結構：
- salaryReport
	- salaryReportAuthentication
	- salaryReportDetails
		- salaryReportCoach
		- salaryReportTherapist
		- salaryReportCounter

這是原本 salaryReport Details 的 beforeEnter 寫法：
```ts
beforeEnter: (to, from) => {
	const salaryReportStore = useSalaryReportStore();
	const userStore = useUserStore();

	if (!salaryReportStore.isAuthenticated) {
		return { name: 'salaryReportAuthentication' };
	}

	const targetRouteName = RoleInfo[userStore.role].salaryRoute;
	return { name: targetRouteName };

	// infinite loop
	// from: salaryReportAuthentication
	// to: salaryReportCoach
}
```

## Analysis

這樣的寫法產生了進入 `salaryReportCoach` 頁面的無限迴圈，我們來分析一下為什麼：
1. 當使用者從 salaryReportAuthentication 驗證後便導向 salaryReportDetails
2. 在進入 salaryReportDetails 前便會先觸發其 beforeEnter hook
	1. 驗證密碼是否輸入（已驗證）
	2. 決定使用者要導向哪裡，當前使用者為教練，因此會導向 salaryReportCoach
3. 由於 salaryReportCoach 為 salaryReportDetails 的子頁面，因此在進入 salaryReportCoach 前，salaryReportDetails 的 beforeEnter 便會先被觸發
	1.  在此次的導航中，其 from 為 salaryReportAuthentication，to 為 salaryReportCoach
	2. 目前仍然為密碼驗證成功的狀態，因此不會導回 salaryReportAuthentication
	3. 根據使用者角色仍然被導向 salaryReportCoach
4. 沒有真正進入 salaryReportCoach，在進入前便又被重新導向至 salaryReportCoach 一次
5. 產生無限迴圈
6. 由於從頭到尾都還沒有進入任何頁面，因此 from 永遠都是 salaryReportAuthentication

## Solution

我們只需要加上一個判斷便可以解決這個問題
- 前來的頁面為 salaryReportAuthentication，而前往的頁面為 salaryReportDetails，才將使用者導向其角色對應的頁面
- 其他時候前往的頁面便不會是 salaryReportDetails，因此我們直接放行讓其前往即可
```ts
beforeEnter: (to, from) => {
	const salaryReportStore = useSalaryReportStore();
	const userStore = useUserStore();

	if (!salaryReportStore.isAuthenticated) {
		return { name: 'salaryReportAuthentication' };
	}

	// avoid infinite loop
	if (from.name === 'salaryReportAuthentication' && to.name === 'salaryReportDetails') {
		const targetRouteName = RoleInfo[userStore.role].salaryRoute;
		return { name: targetRouteName };
	}

	// in other scenarios, the direction would not be salaryReportDetails
	// like salaryReportCoach, salaryReportCoach, etc...
	// so there's no necessary to do any redirection, just let it go
}
```