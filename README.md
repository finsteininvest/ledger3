# Ledger3.py: command-line, double-entry accounting in python

I copied the code and this readme from the [original](https://github.com/finsteininvest/ledger/tree/master).

I believe the original author »[mafm](https://github.com/mafm)« has dropped support for the original code. I sent it through [2to3.py](https://docs.python.org/2/library/2to3.html) so that it runs with Python 3.

I will maintain this code - It is in production use for my personal businesses.

**ledger3.py** is a simple, command-line, double-entry accounting
system. It reads transactions written in a simple format from a text
file and produces summary reports as text or an excel spreadsheet.

Because transaction data is stored as text, it can be managed
using a version control system like git. This makes it easy
to maintain an audit trail.

ledger3.py is like John Wiegley's
[Ledger](http://www.ledger-cli.org/), but [simpler](https://github.com/mafm/ledger3.py/blob/master/doc/Ledger.md).

## What it does

ledger3.py reads an input file that's written in a simple file like this:
```
2013-01-01 I began the year with $1000 in my cheque account.
  Assets:Bankwest:Cheque      $1,000
  Equity:OpeningBalances      $1,000

2013-01-05 I bought some groceries and paid using the cheque account.
  Expenses:Food:Groceries    $98.53
  Assets:Bankwest:Cheque    -$98.53

2013-01-10 I bought some petrol, and paid using a credit card.
  Expenses:Motor:Fuel    $58.01
  Liabilities:Bankwest:Visa   $58.01

2013-01-15 I paid my electricity bill.
  Expenses:Electricity    $280.42
  Assets:Bankwest:Cheque  -$280.42

# I checked my bank statement on the 1st of Feb, and this is what it said.
VERIFY-BALANCE 2013-02-01 Assets:Bankwest:Cheque 621.05
```
and produces reports like this:
```
$./ledger3.py examples/sample.transactions --print-balances
   $621.05 Assets:Bankwest:Cheque
 $1,000.00 Equity:OpeningBalances
   $436.96 Expenses
   $280.42   Electricity
    $98.53   Food:Groceries
    $58.01   Motor:Fuel
    $58.01 Liabilities:Bankwest:Visa
```
or this:
```
$./ledger3.py examples/sample.transactions --print-balances --first-date 2013-01-05 --last-date 2013-01-15
 2013-01-05 2013-01-15   Change Account
    $901.47    $621.05 -$280.42 Assets:Bankwest:Cheque
  $1,000.00  $1,000.00    $0.00 Equity:OpeningBalances
     $98.53    $436.96  $338.43 Expenses
          -    $280.42  $280.42   Electricity
     $98.53     $98.53    $0.00   Food:Groceries
          -     $58.01   $58.01   Motor:Fuel
          -     $58.01   $58.01 Liabilities:Bankwest:Visa
```
or this:
```
$./ledger3.py examples/sample.transactions --print-register Expenses
2013-01-05	 $98.53	 $98.53	Expenses:Food:Groceries	I bought some groceries and paid using the cheque account.
2013-01-10	$156.54	 $58.01	Expenses:Motor:Fuel    	I bought some petrol, and paid using a credit card.
2013-01-15	$436.96	$280.42	Expenses:Electricity   	I paid my electricity bill.
```
## Getting started
The program is currently contained in a single python file. You can grab it and try it out like this:
```
# Grab the code
git clone git://github.com/mafm/ledger3.py
cd ledger3.py

# Generate some example reports
./ledger3.py examples/sample.transactions --print-balances
./ledger3.py examples/sample.transactions --print-register Expenses
./ledger3.py examples/sample.transactions --print-register Expenses:Electricity
```

There is documentation outlining how to use the ledger3.py in the
[Introduction](https://github.com/mafm/ledger3.py/blob/master/doc/Introduction.md)
file in the doc folder.

### Requirements

As far as I know, ledger3.py needs only Python 2.7. I am currently
using it with Python 2.7.2+.

It would be nice to package ledger3.py as a pip package, but that
hasn't been done yet.

#### Testing

If you have nosetests installed, you can also run some tests. They are
not comprehensive. If you have coverage installed, you can also look
at the test coverage like this: ```nosetests --with-coverage
--cover-package=ledger```.

## Status

I am using this program on a daily basis to do real work. I believe that what _has_ been implemented is more
or less correct. However, this program hasn't been extensively tested,
so use it at your own risk.

Lots of useful easy-to-implement features have not yet been
implemented. I am currently (April 2013) attempting to implement one
or two of the missing features per week.

## Origins

ledger3.py is similar to, and partly inspired by, John Wiegley's Ledger: http://www.ledger-cli.org/.

ledger3.py is also similar to some older double-entry accounting
software I wrote using wxPython in 2004. Although that program had a GUI, and
I used it for nearly ten years, it was more complex than ledger3.py,
and I found it less convenient to use.
