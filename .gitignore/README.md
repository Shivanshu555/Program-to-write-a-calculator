# Program-to-write-a-calculator
#These are some fixed value to reduce working
#The first value of the first list in the the list of list is the main unit of prediction
#i.e ['kg',1] in this the output will be in kg

fixed_value={'mass':[['kg',1],['kilograms',1],['kilogram',1],['g',0.001],['grams',0.001],['gram',0.001]],
             'weight':[['N',1],['newton',1],['newtons',1],['n',1]],
             'initial velocity':[['m/s',1],['km/hr',0.27],['km/s',1000],['m/hr',0.01666],['miles/hr',0.44704]],
             'final velocity':[['m/s',1],['km/hr',0.27],['km/s',1000],['m/hr',0.01666],['miles/hr',0.44704]],
             'acceleration':[['m/s^2',1],['km/hr^2',7.716049382716049e-05]],
             'time':[['sec',1],['second',1],['sec',1],['s',1],['min',60],['minute',60],['minutes',60],['hrs',3600],['hours',3600],['hour',3600],['hr',3600]],
             'distance':[['metre',1],['meter',1],['meters',1],['m',1],['km',1000],['kms',1000],['kilometre',1000],['kilometer',1000],['kilometres',1000],['miles',0.000621371],['mile',0.000621371]],
             'displacement':[['metre',1],['meter',1],['meters',1],['m',1],['km',1000],['kms',1000],['kilometre',1000],['kilometer',1000],['kilometres',1000],['miles',0.000621371],['mile',0.000621371]],
             'force':[['N',1],['newton',1],['newtons',1],['n',1]],
             }
             
def data():
        main=[]
        accepted=[]
        #number of values in program
        number=int(input('How many  values are there: '))
        for i in range(number):
                a=i+1

                #collecting data for every single input variable
                v=input('\nEnter the name of value {}: '.format(a))
                s=input('Enter a symbol for {}: '.format(v))
                if v not in fixed_value:
                        cv=input('Enter accepted unit for it: ')
                        mv=input('Enter the main unit of prediction: ')
                else:
                        mv=fixed_value[v.lower()][0][0]
                        
                main.append([s,v,mv])
                print('')
                if v.lower() not in fixed_value:
                        for x in cv.split(','):
                                #data for conversion of values
                                if mv==x:
                                        accepted.append([x,1])
                                        continue
                                
                                unit=float(input('Enter the unit to  convert {1} to {0}: '.format(mv,x)))
                                accepted.append([x,unit])
                else:
                        for x in fixed_value[v.lower()]:
                                accepted.append([x[0],x[1]])
                
        #main equation on which the program works
        equation=[]
        print('')
        for i in main:
           eq=input('Enter equation to get {}: '.format(i[1]))
           for a in eq.split(','):
               equation.append(str(i[0])+'='+str(a))

        return main,accepted,equation
def modify(dataa):
        data=dataa
        main,accepted,equation=data

        #dictionary for converting values
        symbol_change={}
        for i in accepted:
                symbol_change[i[0]]=i[1]

        #dictionary for getting full name and unit from symbol
        fullname={}

        for i in main:
                fullname[i[0]]=(i[1],i[2])

        #inputv function takes all the inputs
        inputv='''def inputv():
        input_data={}'''

        #converter uses the symbol dictionary to convert values
        converter='''def converter(value):'''
        converter+='''
        main={0}
        try:
                v,u=value.split(' ')
                value=float(v)*main[u]
        except:
                value=value*1
        return value '''.format(symbol_change)

        #implementing values in inputv function
        for values in main:
                inputv+='''\n        {0}=input('Enter {1}: ')'''.format(values[0],values[1])
                inputv+='''\n
        {0}={0}.lower()
        if {0}:
                {0}=converter({0})\n
        else:
                {0}=str('?')
        input_data['{0}']={0}
'''.format(values[0])

        #analyzer to analyze the input data and give it a special format

        analyzer='''
def analyzer(input_data,equations):
        data=[]
        full={0}
        for eq in equations:
                a,b=eq.split('=')
                if input_data[a]=='?':
                        for i in b:
                                if i in input_data.keys():
                                        b=b.replace(i,str(input_data[i]))
                        data.append(full[a][0])
                        data.append(eval(b))
                        data.append(full[a][1])
        return list(data)

        '''.format(fullname)
        #implementing analyzer in the inputv function
        inputv+='''
        analyzed_data=analyzer(input_data,{0})
        print('')
        print(str(analyzed_data[0]),':',str(analyzed_data[1])+' '+str(analyzed_data[2]))
        start()'''.format(equation)

        #start function is defined here to restart the program
        start=r'''

def start():
        yn=input('\nDo you want to calculate once again [y/n]: ')
        yn=yn.lower()
        if yn=='y':
                inputv()
        elif yn=='n':
                exit()
        elif yn=='yes':
                inputv()
        elif yn=='no':
                exit()
        else:
                print('Sorry I could not understand')
                start()

'''
        #putting all things together
        full='from math import *\n'+analyzer+'\n'+converter+'\n\n'+inputv+start+'\ninputv()'
        return full
def writer():
        #collecting the data
        datax=modify(data())
        print('')
        #name of the file to implement the data
        name=input('Enter the name for the file: ')
        with open(name+'.py','w+') as f:
                f.write(datax)
writer()

