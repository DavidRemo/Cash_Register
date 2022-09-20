# Cash_Register
### JavaScript


Design a cash register drawer function checkCashRegister() that accepts purchase price as the first argument (price), payment as the second argument (cash), and cash-in-drawer (cid) as the third argument.  

cid is a 2D array listing available currency.  
  
The checkCashRegister() function should always return an object with a status key and a change key.  
  
Return {status: "INSUFFICIENT_FUNDS", change: []} if cash-in-drawer is less than the change due, or if you cannot return the exact change.  
  
Return {status: "CLOSED", change: [...]} with cash-in-drawer as the value for the key change if it is equal to the change due.  
  
Otherwise, return {status: "OPEN", change: [...]}, with the change due in coins and bills, sorted in highest to lowest order, as the value of the change key.  
  
### Currency-Unit	&emsp; Amount  
Penny &emsp; &emsp;	$0.01 (PENNY)  
Nickel &emsp; &emsp;	$0.05 (NICKEL)  
Dime &emsp; &emsp;	$0.1 (DIME)  
Quarter &emsp; &emsp;	$0.25 (QUARTER)  
Dollar &emsp; &emsp;	$1 (ONE)  
Five Dollars &emsp; &emsp;	$5 (FIVE)  
Ten Dollars &emsp; &emsp;	$10 (TEN)  
Twenty Dollars &emsp; &emsp;	$20 (TWENTY)  
One-hundred Dollars &emsp;	$100 (ONE HUNDRED)  
  
See below for an example of a cash-in-drawer array:  
  
[  
&emsp;  ["PENNY", 1.01],  
&emsp;  ["NICKEL", 2.05],  
&emsp;  ["DIME", 3.1],  
&emsp;  ["QUARTER", 4.25],  
&emsp;  ["ONE", 90],  
&emsp;  ["FIVE", 55],  
&emsp;  ["TEN", 20],  
&emsp;  ["TWENTY", 60],  
&emsp;  ["ONE HUNDRED", 100]  
]

### Solution  
  
const currencyUnit = {  
&ensp;  "PENNY": 1,  
&ensp;  "NICKEL": 5,  
&ensp;  "DIME": 10,  
&ensp;  "QUARTER": 25,  
&ensp;  "ONE": 100,  
&ensp;  "FIVE": 500,  
&ensp;  "TEN": 1000,  
&ensp;  "TWENTY": 2000,  
&ensp;  "ONE HUNDRED": 10000  
}  
  
function checkCashRegister(price, cash, cid) {  
  
&ensp;  let changeSum = cash * 100 - price * 100;  
&ensp;  let changeSumCheck = changeSum;  
&ensp;  let change = [];  
&ensp;  let status = '';  
  
&ensp;  let cidSum = 0;  
&ensp;  let filterdCid = cid.filter(elem => elem[1] !== 0).reverse();  
  
&ensp;  filterdCid.forEach(elem => {  
&emsp;    let curr = elem[0];  
&emsp;    let currSum = elem[1] * 100;  
&emsp;    cidSum += currSum;  
&emsp;    let amount = 0;  
&emsp;    while (changeSum >= currencyUnit[curr] && currSum > 0) {  
&emsp;      amount += currencyUnit[curr];  
&emsp;      changeSum -= currencyUnit[curr];  
&emsp;      currSum -= currencyUnit[curr];  
&emsp;    }  
&emsp;    if (amount !== 0) {  
&emsp;      change.push([curr, amount / 100]);  
&emsp;    }  
&ensp;  });  
  
&ensp;  if (changeSum > 0){  
&emsp;    status = 'INSUFFICIENT_FUNDS';  
&emsp;    change = [];  
&ensp;  } else if (changeSum == 0 && changeSumCheck == cidSum) {  
&emsp;    status = 'CLOSED';  
&emsp;    change = cid;  
&ensp;  } else {  
&ensp;    status = 'OPEN';  
&ensp;  }  
   
&ensp;  return { 'status': status, 'change': change };  
  
}  
  
console.log (checkCashRegister(19.5, 20, [["PENNY", 1.01], ["NICKEL", 2.05], ["DIME", 3.1], ["QUARTER", 4.25], ["ONE", 90], ["FIVE", 55], ["TEN", 20], ["TWENTY", 60], ["ONE HUNDRED", 100]]));  
  
  
### Output  
  
{ status: 'OPEN', change: [ [ 'QUARTER', 0.5 ] ] }
