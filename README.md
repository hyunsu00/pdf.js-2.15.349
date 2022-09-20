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


### 디버그 정보
```javascript
//
// src/core/annotation.js
class Annotation
	- constructor(params)
	- async getOperatorList(
		evaluator,
		task,
		intent,
		renderForms,
		annotationStorage)

//
// src/core/primitives.js
class Dict
	- set(key, value)

//
// src/core/evaluator.js
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
// src/core/operator_list.js
class OperatorList
	- addOp(fn, args)

//
// web/app.js
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

### 저장시 Sequence 다이어그램

```javascript
// 1. web/app.js
downloadOrSave() {
	...
	this.save();
	...
}
async save() {
	...
	const data = await this.pdfDocument.saveDocument();
	...
}

// 2. src/core/worker.js
// WorkerMessageHandler.createDocumentHandler
static createDocumentHandler(docParams, port) {
	...
	handler.on(
		  "SaveDocument",
		  function ({ isPureXfa, numPages, annotationStorage, filename }) {
			...
			return page
                  .saveNewAnnotations(handler, task, annotations)
			...
		  }
	);
	...
}

// 3. src/core/document.js
// Page.saveNewAnnotations
async saveNewAnnotations(handler, task, annotations) {
	...
	const newData = await AnnotationFactory.saveNewAnnotations(
      partialEvaluator,
      task,
      annotations
    );
	...
	writeObject(this.ref, pageDict, buffer, transform);
	...
}

// 4. src/core/annotation.js
// AnnotationFactory.saveNewAnnotations
static async saveNewAnnotations(evaluator, task, annotations) {
	...
	switch (annotation.annotationType) {
		...
		FreeTextAnnotation.createNewAnnotation(...)
		...
		InkAnnotation.createNewAnnotation(...)
		...
	}
	...
}
// MarkupAnnotation.createNewAnnotation
static async createNewAnnotation(xref, annotation, dependencies, params) {
	...
	writeObject(...);
	...
	writeObject(...);
}

// 5. src/core/writer.js
function writeObject(ref, obj, buffer, transform) {
	...
}
```
