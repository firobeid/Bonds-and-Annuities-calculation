# Bonds-and-Annuities-calculation
Assignment to calculate several questions that cover the fixed income and discounted cash flows in the form of annuities. 
# -*- coding: utf-8 -*-
"""
Spyder Editor

Author: Firas Obeid
Email: feras.obeid@lau.edu
"""

from __future__ import division
# imported from division library in order to execute division of integers and floats in version 2.7
# The use of functions previals in this code, and some variables are used in different functions
# Functions have local variables, thus they wont affect other function variables
# Functions can give the answers either by dictionaries in them (DCF part)
# Or pass parameters to arguments as in the Bonds section
print('\t\t\t Discounted Cash Flow')
# Question 1
def cash_flow():
    annuity = {"PV": 31000, "i": 6.25 / 100,"k": 1,"n": 14}
    y = (1 + (annuity["i"] / annuity["k"])) 
    payments = (annuity['PV']) * ((y ** annuity["n"]) * (y - 1)) / ((y ** annuity["n"] - 1))
    return payments
pmts = cash_flow()
print("\n\n Question 1:\n The annual cash flows on the annuity will be {}".format("${:,.2f}".format(pmts)) + ".")
# Question 2
def perpetuity():
    perpetuity_dictionary = {"pmt": 1200, "PV": 61000, "k": 12, "n": "infinity"}
    monthly_return = (perpetuity_dictionary["pmt"] / perpetuity_dictionary["PV"]) * 100
    APR = perpetuity_dictionary["k"] * monthly_return
    EAR = (((1 + ((APR / 100) / perpetuity_dictionary["k"])) ** perpetuity_dictionary['k']) - 1) * 100
    return monthly_return, APR, EAR 
r, APR, EAR = perpetuity() #Calling the functions with returned variables; The variables can be different but be off the same order of those returned
print("\n Question 2:\n a) The monthly return on the investment is {}%\n b) The APR (annual percentage rate) is {}%\
    \n c) The EAR (effective annual rate) is {}%".format(r, APR, EAR))
# Question 3
# formula for this function is FV = PV(1+R/K)**kt but we need r thus we do algebric expansion in terms of r
def rate():
    investment = {"FV/PV": 3, "k": 4, "years": 6} #FV/PV is 3 since we are trippling our investment
    r = (investment["k"] * ((investment["FV/PV"] ** (1 / (investment["k"] * investment["years"])) - 1))) * 100   
    return r
quarter_rate = round(rate() / 4, 4)
print("\n Question 3:\n The rate of return per quarter offered is {}%".format(quarter_rate))
# Question 4
def present_value():
    annuity = {"pmt": 540000, "r": 10 / 100, "g": 5 / 100, "n": 16}
    factor = ((1 + annuity["g"]) / (1 + annuity["r"])) # Growing annuity has a growth rate g
    value = ((annuity["pmt"] / (annuity['r'] - annuity['g'])) * (1 - factor**(annuity['n'])))
    return value
PV = present_value()
print("\n Question 4: \n The present value of the winnings is {}".format("${:,.2f}".format(PV)))
# Question 5
import math # The following formula will undergo algebraic expnsion and will require the natural log(ln thus we import the math module)
def periods():
    S_O_A = {"FV": 25694, "pmt": 380, "r": 8 / 100, "k": 12}
    y = 1 + (S_O_A["r"] / S_O_A["k"])
    n = math.log(((S_O_A["FV"] * (S_O_A["r"] / S_O_A["k"] )) / S_O_A["pmt"]) + 1) / math.log(y)
    return n
payments = periods() # n in returned is the same as variable payments
print("\n Question 5: \n The number of payments made is {} pmts when the account balance reaches $25,694".format(payments))
# Question 6
def balloon_payments():
    loan = {"PV": 230000, "n": (30 * 12), "r": 7.6 / 100, "k": 12, "pmt2": 800} # the loan should be paid with 1623.9719 worth of monthly payments
    y = 1 + (loan['r'] / loan['k'])                                        # But since only 800 worth thus we calculate new PV of such loan
    present_value_800 = ((1 - (y ** -loan['n'])) / (y - 1)) * loan["pmt2"]  # Then amount owed in present value is discounted to find the future value balloon payment owed after loan payments end
    still_owe = loan["PV"] - present_value_800
    still_owe_future_value = still_owe * (y ** loan["n"])
    owe_with_last_payment = still_owe_future_value + 800 # This variable is to take into account the last monthly payment if it is not payed separately
    return still_owe_future_value, owe_with_last_payment
payment, payment_plus = balloon_payments()
print("\n Question 6: \n The balloon payment has to be ${:,.2f}.".format(payment) +" However if the last monthly payment \
is not payed separatly from the balloon payment, then the balloon payment becomes ${:,.2f}.".format(payment_plus))
 
# Question 7: imported numpy to solve for the rate of a loan
import numpy as np
loan = {"n": 360, "pmt": -17000, "PV loan": 2800000 * 0.8, "FV": 0.0, "k": 12}
r = (np.rate(loan['n'], loan['pmt'], loan['PV loan'], loan['FV'])) * 100
APR = round(r * 12, 4)
EAR = round((((1 + ((APR / 100) / loan['k'])) ** loan['k']) - 1) * 100, 4)
print("\n Question 7: \n a) The APR on the loan is {}% \n b) The EAR on the loan is {}%".format(APR, EAR))

# Question 8    
def PV_annuity(): # The two stages are in intention to bring all future values to the present value. The second stage will be the FV of the first stage 
    annuity = {"n": 16, "pmt": 1700, "n1": 6 * 12, "n2": 10 * 12, "r1": 13 / 100, "r2": 10 / 100, "k": 12}  # and discounted by the rate of the first stage to bring both to present
    y2 = 1 + (annuity["r2"] / annuity["k"])
    y1 = 1 + (annuity["r1"] / annuity["k"])
    PV_of_2nd_period = ((annuity["pmt"]) * (1 - (y2) ** (-annuity["n2"])) / (y2 - 1)) / (y1 ** annuity["n1"])
    PV_of_1st_period = ((annuity["pmt"]) * (1 - (y1) ** (-annuity["n1"]))) / (y1 - 1)
    PV_annuity = PV_of_2nd_period + PV_of_1st_period
    return PV_annuity
total_PV = PV_annuity()
print("\n Question 8: \n The present value of the the two stage annuity is {}".format("${:,.2f}".format(total_PV)))
# Question 9    
def annuity_due():
    annuity = {"PV": 43000, "n": 60, "k": 12, "r": 8.25 / 100}
    # Since this is an annuity due then in the payment formula PV ordinary annuity = PVdue / (1+r)  
    y = 1 + (annuity['r'] / annuity['k'])
    payment_due = (annuity["PV"] / y) * ((y-1) / (1 - (y) ** -annuity['n']))    
    return payment_due
monthly_pmt = round(annuity_due(), 2)
print("\n Question 9: \n The monthly payment on the the annuity due is ${}".format(monthly_pmt))

# Question 10
def loan_rate(amount, k, points, r):
    loan_recieved = amount * (1 - points)
    loan_repayment = amount * (1 + r) ** k
    rate_actual = round(((loan_repayment / loan_recieved) ** (1 / k) - 1) * 100, 4)
    return rate_actual
rate = loan_rate(5000, 1, 3 / 100, 10 / 100)
print("\n Question 10: \n The actual rate paid on the loan is {}%".format(rate))
# BONDS QUESTIONS
print('\n\n\t\t\t Bonds')
# Question 1
def PV_Bond(n, r, k, coupon_pmt, FV):
    y = 1 + (r / k)
    present_value = (coupon_pmt * (((y) ** n) - 1) / (((y) ** n) * r)) + (FV / (y) ** n)
    return present_value
PV = round(PV_Bond(14, 10 / 100, 1, 1000 * (6 / 100), 1000), 2)
print("\n Question 1: \n The current price of Staind Inc. bond is ${}".format(PV))

# Question 2
import numpy as np #import numpy to use the np.irr function. The When the PV of bond price 
def Rate_Bond(n, coupon_pmt, PV, FV, k): # is taken to other side of numeric equation
    cash_flows = (coupon_pmt / k) * np.ones((n * k) +1)  # the equation becomes = 0 and PV sign is negative
    cash_flows[-1] += FV
    cash_flows[0] = -PV
    rate = round((np.irr(cash_flows) * 100) * k, 4)
    return rate
YTM = Rate_Bond(14, 1000 * (9/100), 850.46, 1000, 1)
print("\n Question 2: \n The YTM on Acherman Co. Bonds is {}%".format(YTM))

# Question 3
def Coupon_Rate(n, PV, r, FV, k):
    y = 1 + (r / k)
    coupon = (PV - ((FV / (y) ** n))) / ((((y) ** n) - 1) / (((y) ** n) * r))
    coupon_rate = (coupon / 1000) * 100
    return coupon_rate
CR = round(Coupon_Rate(6, 970, 9.9 / 100, 1000, 1), 4)
print("\n Question 3: \n The coupon rate on Kiss the Sky Enterprises bond is {}%".format(CR))
# Question 4 
# This is a PV of a semiannual bond, thus n (maturity life) should be * k(semi annual = 2) and CR is divided by k
def PV_Bond(n, r, k, coupon_pmt, FV):
    y = 1 + (r / k)
    present_value = ((coupon_pmt / k) * (((y) ** (n * k)) - 1) / (((y) ** (n * k)) * (r / k))) + (FV / (y) ** (n * k))
    return present_value
PV = round(PV_Bond(16, 12 / 100, 2, 1000 * (8 / 100), 1000), 2)
print("\n Question 4: \n The current price of Grohl Co. bond is ${} since it was issued a year ago".format(PV))
# Question 5
import numpy as np #import numpy to use the np.irr function. The When the PV of bond price 
def Rate_Bond(n, coupon_pmt, PV, FV, k): # is taken to other side of numeric equation
    cash_flows = (coupon_pmt / k) * np.ones((n * k) +1)  # the equation becomes = 0 and PV sign is negative
    cash_flows[-1] += FV
    cash_flows[0] = -PV
    rate = round((np.irr(cash_flows) * 100) * k, 4)
    return rate
YTM = round(Rate_Bond(17, 1000 * (9.4 / 100), 1.02 * 1000, 1000, 2), 4)
print("\n Question 5: \n The YTM on Ngata Corp. Bonds is {}%".format(YTM))
# Question 6
def Coupon_Rate(n, PV, r, FV, k):
    y = 1 + (r / k)
    coupon = (PV - ((FV / (y) ** n))) / ((((y) ** n) - 1) / (((y) ** n) * (r / k)))
    coupon_rate = ((coupon / 1000) * k) * 100
    return coupon_rate
CR = round(Coupon_Rate(17 * 2, 1236.50, 11 / 100, 1000, 2), 4)  #since semiannual k_periodic = 2. Thus we multiply years by k and divide YTM by k. Then we multiply CR by k to annual coupon rate.
print("\n Question 6: \n The coupon rate on Ashes Divide Corporation bond is {}%".format(CR))
# Question 7
rate_calc = {"real_rate": 5.5, "inflation": 2}
TB_rate = rate_calc["real_rate"] + rate_calc["inflation"]
TB_rate_exact = round(((1 + (rate_calc["real_rate"] / 100)) * (1 + (rate_calc["inflation"] / 100)) - 1) *100, 4)
print("\n Question 7: \n The expected treasury rate will be {}%. However, the exact  treasury bill rate will have a \
different calculation and will be {}% for info.".format(TB_rate, TB_rate_exact))
# Question 8
def inflation_r(r_o_r, r_real):
    inflation = round(((r_o_r - r_real) / ( 1 + r_real)) * 100, 4)
    return inflation
inflation = inflation_r(13 / 100, 6.5 / 100)
print("\n Question 8: \n Bill believes that the inflation rate will be {}% next year".format(inflation))

# Question 9
def real_rate(r_o_r, inflation):
    r_real = round(((r_o_r - inflation) / (1 + inflation)) * 100, 4)
    return r_real
r_real = real_rate(9.5 / 100, 5 / 100)
print("\n Question 9: \n The real rate of return after taking inflation into consideration is {}%".format(r_real))
# Question 10
# Coded a function that gives percentage change when YTM moves up/down by 4% for two different periods of bonds
# Passed the parameters to the arguments (kwargs) in a single function with %changes called after function definition
def PV_Bond_Change(n_sam, n_dave, coupon_pmt, r_increase, r_decrease, FV, k):
      y_increase = 1 + (r_increase / k)
      y_decrease = 1 + (r_decrease / k)
      PV_orginal_sam = FV
      PV_orginal_dave = FV
      PV_increase_sam = (coupon_pmt * (((y_increase) ** n_sam) - 1) / (((y_increase) ** n_sam) * r_increase)) + (FV / (y_increase) ** n_sam)
      PV_increase_dave = (coupon_pmt * (((y_increase) ** n_dave) - 1) / (((y_increase) ** n_dave) * r_increase)) + (FV / (y_increase) ** n_dave)
      PV_decrease_sam = (coupon_pmt * (((y_decrease) ** n_sam) - 1) / (((y_decrease) ** n_sam) * r_decrease)) + (FV / (y_decrease) ** n_sam)
      PV_decrease_dave = (coupon_pmt * (((y_decrease) ** n_dave) - 1) / (((y_decrease) ** n_dave) * r_decrease)) + (FV / (y_decrease) ** n_dave)
      P_increase_sam = ((PV_increase_sam - PV_orginal_sam) / PV_orginal_sam) * 100
      P_increase_dave = ((PV_increase_dave - PV_orginal_dave) / PV_orginal_dave) * 100
      P_decrease_sam = ((PV_decrease_sam - PV_orginal_sam) / PV_orginal_sam) * 100 # P_decrease_sam 
      P_decrease_dave = ((PV_decrease_dave - PV_orginal_dave) / PV_orginal_dave) * 100
      return  P_increase_sam,  P_increase_dave, P_decrease_sam, P_decrease_dave
P_increase_sam,  P_increase_dave, P_decrease_sam, P_decrease_dave =  PV_Bond_Change(3 * 2, \
                                              18 * 2, \
                                              1000 * (6 / 100), \
                                              (6 + 4) / 100, \
                                              (6 - 4) / 100, \
                                              1000, 2) # used \ operator to go down in line in the same code 
print("\n Question 10: \n a) If interest rates rise by 4%, the %change in Sam's bond will be {}% \
      \n b) If interest rates rise by 4%, the %change in Dave's bond will be {}%. \
\n c) If interest rates drop by 4%, the %change in Sam's bond will be {}%. \
\n d) If interest rates drop by 4%, the %change in Dave's bond will be {}%. \
\n This proves that that longer the bond period horizon the more sensitive it is to \
yields, interest rate, changes and flucations. \n\n\n \t\t\t\t ----END----".format(P_increase_sam,  P_increase_dave, P_decrease_sam, P_decrease_dave))



    
