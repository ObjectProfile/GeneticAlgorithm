# GeneticAlgorithm

Export code snipped

```
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
