I am going to describe cashflow stacking. I will first describe all the variables in cashflow stacking.

`personal_income`: how much money you earn each month. Default value is $10,000.

`personal_expenses`: how much you spend on bills each month. Default value is $8,000.

`personal_cashflow`: This is the difference between `personal_income` and `personal_expenses`. 
E.g. if your `personal_income` is $5000 and your `personal_expenses` are $4500 
then your `personal_cashflow` is $500.

`paydown_time`: This is the maximum amount of months you want to spend to repay borrowed capital.
5 is a typical default value for `paydown_time`. You dont want to spend 10 months paying back 
borrowed capital and it is not realistic to want to pay it back in 2-3 months. 
Most people aim for a `paydown_time` of 4 or 5 months.

`flip_size`: The `flip_size` is the amount of capital that you borrow with the intent of paying 
it down within a certain number of months or slightly less. 
Here is how you calculate `flip_size` initially: personal_cashflow * 5. 

`amortization_years`: the number of years that an amortized investment pays. 
This value is typically 3, but could be 2.

`amortization_percentage`: this integer shows the percent return of the investment.

`monthly_roi`: this is the expected monthly amortized return of the investment. 
The overall_return is `amortization_percentage` over `amortization_years`. 
The monthly_roi is a monthly payment of that overall_return. 
For instance, if `flip_size` is $10,000, then you will buy an amortized investment with that 
$10,000 and it will make monthly returns on investment for `amortization_years`. 
Therefore, if the amortization_percentage is 12, then monthly payments of $332.14 can be expected 
for 3 years because $10,000 amortized at an interest rate of 12% for 3 years leads to that monthly_roi, 
as this shows - https://tinyurl.com/pkbwfsx2  ... you are expected to use a true amortizing loan formula, 
e.g: P * r / (1 – (1+r)^(-n)) …where r = annual_interest/12, n = years*12. 
Note well: the `monthly_roi` for a new investment is based on the current `flip_size` at time of purchase.
So later investments could have higher monthly ROI if `flip_size` has grown.


Now that I have described the variables, I want you to produce a spreadsheet. Here are the columns in the spreadsheet:
- `date`: Start at 1/1/2025. Each row represents 1 month.
- `personal_cashflow`: a constant calculated from personal_income and personal_expenses
- `stacked_roi`: this is sum of the roi of all purchased amortized investments. Remember, each time we buy an investment, we expect `monthly_roi` from that investment for `amortization_years`. Therefore as we buy investments, the `monthly_roi` from each investment will "stack" leading to this column being the sum of all monthly purchased investments.
- `total_cashflow`
- `borrowed`: Whenever you borrow `flip_size`,  borrowed += flip_size. And each month as you reduce `borrowed` by `total_cashflow` it reduces.
- `stack_number`: each time you buy an investment, this number increases
- `flip_size`
- `monthly_roi`: the monthly roi expected from this stack
- `stack_years`: this is a synonym for amortization_years



This is what you do each month:
1. stacked_roi = sum of monthly_roi for all investments still paying (each lasts 36 months from purchase date).
2. total_cashflow = personal_cashflow + stacked_roi. Duplicate the value`total_cashflow` into `this_months_cashflow`.
3. Check flip_size increase: if total_cashflow ≥ (flip_size × 1.5) / paydown_time then flip_size = flip_size × 1.5.
4. Check borrow opportunity: if borrowed < this_months_cashflow, borrow flip_size and buy an investment that starts paying next month. Increment stack counter, record date, stack_years = 3, monthly_roi for that stack = amortized payment based on current flip_size.
5. Pay down borrowed: borrowed = max(0, borrowed - total_cashflow).

But note well: in month 1, you only perform step 4 above.

The final thing you need to know is what years of the cashflow stacking to produce. They may just want a specific year. Or perhaps they may want a range of years. By default, show the the first 2 years.

