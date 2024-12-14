# 📈 YTMFinder
Find easily the yield to maturity of any bond.

[ING Bank N.V. Senior Notes](https://www.ing.com/Investors/Fixed-income-information/Debt-securities-ING-Bank-N.V./Senior-bonds.htm)  
On the website above, you can find the Senior Notes of ING Bank N.V.

## 📘 Details
**Issuer**: ING Bank N.V.  
**Tranche (1)**: 1,000,000,000 EUR  
**Tenor**: 3 Years  
**Coupon**: 4.125%  
**Issue Price**: 99.903% of Nominal Value (Tranche (1))  

---

## ⚙️ Input Values
```python
Nominal = 1000000000  # Face value of the bond
price = 99.903 * 0.01 * Nominal  # Price of the bond
Coupon = 4.125 * 0.01 * Nominal  # Annual coupon payment
Tenor = 3  # Number of years to maturity
Y = 0.04  # Initial guess for YTM

# Calculate the CashFlows Present Value and their discrepancy with the actual price
def discounted_cashflow(Y):
    return (
        Coupon / (1 + Y) +
        Coupon / (1 + Y)**2 +
        Coupon / (1 + Y)**3 +
        Nominal / (1 + Y)**3
    )

PV = discounted_cashflow(Y)

print("Present Value - Price: ", PV - price)

# Adjust the initial guess for the YTM so that the price equals PV of cash flows
if PV == price:
    print(f"Yield to Maturity (YTM): {Y}")
else:
    while abs(PV - price) > 10000:  
        if PV < price:
            Y = Y - 0.0001
            PV = discounted_cashflow(Y)
            print("Present Value - Price: ", PV - price, "YTM: ", Y)
        else:
            Y = Y + 0.0001  
            PV = discounted_cashflow(Y)
            print("Present Value - Price: ", PV - price, "Yield To Maturity: ", Y)

