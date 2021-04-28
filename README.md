# GeneticAlgorithm

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
			at: DateAndTime putName: 'Timestamp' namespace: 'Core';
			at: AssertionFailure putName: 'Error' namespace: 'Core'.
		];
	export.
```

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
