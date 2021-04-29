# GeneticAlgorithm

This repository contains the parcels for VisualWorks 7.4 and 8.x from the pharo project GeneticAlgorithm, use parcel manager to install it.


# Examples

The following example uses a sudoku board, and It finds the correct position for each cell in a matrix of 3x3 where the sum of each row, column diagonal should be 30.

```Smalltalk
| list sums g |
list := #(2 4 6 8 10 12 14 16 18).

"The different combinations to sum.
E.g., the three first cells could be summed (#(1 2 3))
	  the diagonal top-left to bottom-right (#1 5 9))"
sums := #( 
	#(1 2 3)
	#(4 5 6)
	#(7 8 9)
	
	#(1 5 9)
	#(7 5 3)
	
	#(1 4 7)
	#(2 5 8)
	#(3 6 9) ).
g := GAEngine new.
g populationSize: 400.
g endIfFitnessIsAbove: 9.
g mutationRate: 0.01.
g numberOfGenes: 9.
g createGeneBlock: [ :rand :index | list atRandom: rand. ].
g fitnessBlock: [ :genes |
	| score penalty |
	score := (sums collect: [ :arr |
			(arr collect: [ :index | genes at: index]) sum ]) 
				inject: 0 into: [ :a :b | a + (b - 30) abs ].
	penalty := (genes size - genes asSet size) * 3.
	9 - (score + penalty) ].
g run.
g inspect.
```

# Depedencies
https://github.com/Apress/agile-ai-in-pharo
https://github.com/ObjectProfile/Pharo2VW

# Exporting steps

Export code snipped

```Smalltalk
Pharo2VW exporter
	"directory: '.' asFileReference;"
	namespace: 'GeneticAlgorithm';
	methodsBlacklist: {};
	addNewInitializerToSuperclasses: true;
	packages: {'GeneticAlgorithm-Core'. 'GeneticAlgorithm-Tests'};
	classNameMapperDo: [ :m |
		m 
			at: AssertionFailure putName: 'Error' namespace: 'Core';
			at: DateAndTime putName: 'Timestamp' namespace: 'Core';
			at: AssertionFailure putName: 'Error' namespace: 'Core'.
		];
	export.
```

The result should print some thing like this

![ga result](https://github.com/akevalion/GeneticAlgorithm/raw/main/img/ga%20result.jpg?raw=true)



Then in VW execute to sort

```Smalltalk
| main classes cat pkg packages |
main := Registry bundleNamed: 'GeneticAlgorithm'.
classes := main allClasses.
packages := Dictionary new.
classes do: [ :cls |  
	cat := cls myClass category asString.
	pkg := packages at: cat ifAbsentPut: [ | p |
		p := Registry packageNamedOrCreate: cat.
		main addItem: p.
		p ].
	XChangeSet current moveWholeClass: cls toPackage: pkg
	]
```
After that you will need to create extension for vw
