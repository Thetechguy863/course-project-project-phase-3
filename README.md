def getempname():
    empname = input("Enter employee name: ")
    return empname

def getdatesworked():
    fromdate = input("Please enter start date in the following format MM/DD/YYYY: ")
    enddate = input("Please enter end date in the following format MM/DD/YYYY: ")
    return fromdate, enddate

def gethoursworked():
    hours = float(input("Enter hours: "))
    return hours

def gethourlyrate():
    hourlyrate = float(input("Enter hourly rate: "))
    return hourlyrate

def gettaxrate():
    taxrate = float(input("Enter tax rate: "))
    taxrate = taxrate / 100
    return taxrate

def calctaxandnetpay(hours, hourlyrate, taxrate):
    gpay = hours * hourlyrate
    incometax = gpay * taxrate
    netpay = gpay - incometax
    return gpay, incometax, netpay

def printinfo(empdetaillist):
    totalemployees = 0
    totalhours = 0.00
    totalgrosspay = 0.00
    totaltax = 0.00
    totalnetpay = 0.00
    for empdetail in empdetaillist:
        fromdate = empdetail[0]
        enddate = empdetail[1]
        empname = empdetail[2]
        hours = empdetail[3]
        hourlyrate = empdetail[4]
        taxrate = empdetail[5]

        grosspay, incometax, netpay = calctaxandnetpay(hours, hourlyrate, taxrate)
        print(fromdate, enddate, empname, f"{hours:,.2f}", f"{hourlyrate:,.2f}", f"{grosspay:,.2f}",
              f"{taxrate:.1%}", f"{incometax:,.2f}", f"{netpay:,.2f}")
        totalemployees += 1
        totalhours += hours
        totalgrosspay += grosspay
        totaltax += incometax
        totalnetpay += netpay

    emptotals["totemp"] = totalemployees
    emptotals["tothours"] = totalhours
    emptotals["totgross"] = totalgrosspay
    emptotals["tottax"] = totaltax
    emptotals["totnet"] = totalnetpay

def printtotals(emptotals):
    print(f'Total number of employees:   {emptotals["totemp"]}')
    print(f'Total hours of employees:   {emptotals["tothours"]:.2f}')
    print(f'Total gross pay of employees:   {emptotals["totgross"]:.2f}')
    print(f'Total tax of employees:   {emptotals["tottax"]:.2f}')
    print(f'Total net pay of employees:   {emptotals["totnet"]:.2f}')

def writeemployeeinformation(employee):
    file = open("employeeinfo.txt", "a")
    file.write('{}|{}|{}|{}|{}|{}\n'.format(employee[0], employee[1], employee[2], employee[3], employee[4], employee[5]))
    file.close()

def getfromdate():
    valid = False
    fromdate = ""

    while not valid:
        fromdate = input("Enter from date (m/dd/yyyy): ")
        if len(fromdate.split('/')) != 3 and fromdate.upper() != 'ALL':
            print("Invalid date format")
        else:
            valid = True
    return fromdate

def reademployeeinformation(fromdate):
    empdetaillist = []

    file = open("employeeinfo.txt", "r")
    data = file.readlines()

    condition = True

    if fromdate.upper() == 'ALL':
        condition = False

    for employee in data:
        employee = [x.strip() for x in employee.strip().split('|')]

        if not condition:
            empdetaillist.append([employee[0], employee[1], employee[2], float(employee[3]), float(employee[4]), float(employee[5])])
        else:
            if fromdate == employee[0]:
                empdetaillist.append([employee[0], employee[1], employee[2], float(employee[3]), float(employee[4]), float(employee[5])])
    file.close()
    return empdetaillist

if __name__ == "__main__":
    empdetaillist = []
    emptotals = {}
    
    while True:
        empname = getempname()
        if empname.upper() == "END":
            break

        fromdate, enddate = getdatesworked()
        hours = gethoursworked()
        hourlyrate = gethourlyrate()
        taxrate = gettaxrate()

        empdetail = [fromdate, enddate, empname, hours, hourlyrate, taxrate]
        writeemployeeinformation(empdetail)

    print()
    print()
    fromdate = getfromdate()

    empdetaillist = reademployeeinformation(fromdate)
    print()
    printinfo(empdetaillist)

    print()
    printtotals(emptotals)


