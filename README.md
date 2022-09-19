# pdf.js-2.15.349

## 빌드

```bash
npm install -g gulp-cli
npm install --ignore-scripts --force
gulp locale
gulp server
```

## 삭제

```bash
gulp clean
```


```javascript
//
// src\core\annotation.js
class Annotation
	- constructor(params)
	- async getOperatorList(
		evaluator,
		task,
		intent,
		renderForms,
		annotationStorage)

//
// src\core\primitives.js
class Dict
	- set(key, value)

//
// src\core\evaluator.js
class PartialEvaluator
	- getOperatorList({
		stream,
		task,
		resources,
		operatorList,
		initialState = null,
		fallbackFontDict = null,
	  })

//
// src\core\operator_list.js
class OperatorList
	- addOp(fn, args)

//
// web\app.js
PDFViewerApplication
	- downloadOrSave() {
		// if (this.pdfDocument?.annotationStorage.size > 0) {
		//   this.save();
		// } else {
		//   this.download();
		// }
		this.save();
	  },
```
