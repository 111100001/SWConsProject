<div dir="rtl">

## FunctionDef as_bytes(bytes_or_text, encoding)
**as_bytes**: وظيفة `as_bytes` هي تحويل أنواع الإدخال في بايثون مثل `bytearray`، `bytes`، أو النصوص Unicode إلى نوع `bytes`.

**parameters**: معاملات هذه الوظيفة.
· `bytes_or_text`: كائن من نوع `bytearray`، `bytes`، `str`، أو `unicode`.
· `encoding`: سلسلة نصية تشير إلى مجموعة الأحرف المستخدمة في ترميز النصوص Unicode (الافتراضي هو `utf-8`).

**Code Description**: 
تقوم هذه الوظيفة بتحويل أنواع البيانات المختلفة إلى نوع `bytes`. يتم ذلك عن طريق التحقق من نوع البيانات المدخلة (`bytes_or_text`) ثم تحويلها وفقًا لذلك:
- إذا كان النوع `bytearray`، يتم تحويله مباشرة إلى `bytes`.
- إذا كان النوع `str` أو `unicode`، يتم ترميزه باستخدام الترميز المحدد (`encoding`) وتحويله إلى `bytes`.
- إذا كان النوع `bytes` بالفعل، يتم إرجاعه كما هو.
- إذا كان النوع غير معروف أو غير مدعوم، يتم إطلاق استثناء `TypeError`.

في المشروع، يتم استدعاء هذه الوظيفة من خلال وظيفة أخرى تسمى `path_to_bytes`، والتي تقوم بتحويل كائنات `PathLike` أو `str` إلى `bytes`. يتم ذلك عن طريق استدعاء `as_bytes` بعد التأكد من أن المدخل يمكن تحويله إلى مسار. هذه العلاقة توضح أن `as_bytes` تعمل كوظيفة مساعدة لتحويل البيانات النصية أو الثنائية إلى `bytes`، مما يجعلها جزءًا أساسيًا في معالجة المسارات والبيانات النصية في المشروع.

**Note**: 
- يجب التأكد من أن المدخل (`bytes_or_text`) إما نص Unicode أو بيانات ثنائية (`bytes` أو `bytearray`)، وإلا سيتم إطلاق استثناء `TypeError`.
- الترميز الافتراضي هو `utf-8`، ولكن يمكن تغييره باستخدام المعامل `encoding`.

**Output Example**: 
إذا تم استدعاء الوظيفة بالمدخل التالي:
```python
as_bytes("مرحبًا")
```
فإن الناتج سيكون:
```python
b'\xd9\x85\xd8\xb1\xd8\xad\xd8\xa8\xd9\x8b\xd8\xa7'
```
وهذا يمثل النص "مرحبًا" مُرمَّزًا باستخدام `utf-8`.
## FunctionDef as_text(bytes_or_text, encoding)
**as_text**: وظيفة `as_text` هي تحويل أي نوع من أنواع النصوص أو البيانات الثنائية (bytes) في بايثون إلى نص من نوع `unicode`.

**parameters**: معاملات هذه الوظيفة.
· `bytes_or_text`: يمكن أن يكون من نوع `bytes`، أو `str`، أو `unicode`. هذا هو المدخل الذي سيتم تحويله إلى نص من نوع `unicode`.
· `encoding`: سلسلة نصية تحدد ترميز النص المستخدم لفك التشفير. القيمة الافتراضية هي `utf-8`.

**Code Description**: 
تقوم هذه الوظيفة بتحويل أي مدخل نصي أو بيانات ثنائية إلى نص من نوع `unicode`. يتم ذلك عن طريق التحقق أولاً من صحة الترميز المحدد (`encoding`) باستخدام `codecs.lookup`. إذا كان المدخل (`bytes_or_text`) بالفعل من نوع `unicode` (أو `str` في بايثون 3)، يتم إرجاعه كما هو. إذا كان المدخل من نوع `bytes`، يتم فك تشفيره باستخدام الترميز المحدد وإرجاعه كنص من نوع `unicode`. في حالة عدم تطابق المدخل مع أي من هذه الأنواع، يتم إطلاق خطأ من نوع `TypeError`.

في المشروع، يتم استدعاء هذه الوظيفة من خلال وظيفة أخرى تسمى `as_str`، والتي تعيد ببساطة نتيجة `as_text`. هذا يعني أن `as_str` تعمل كواجهة مبسطة لـ `as_text`، حيث تقوم بنفس العملية ولكن بدون الحاجة إلى تحديد الترميز بشكل صريح في كل مرة.

**Note**: 
- يجب التأكد من أن المدخل (`bytes_or_text`) إما من نوع `bytes` أو `unicode`/`str`، وإلا سيتم إطلاق خطأ `TypeError`.
- الترميز الافتراضي هو `utf-8`، ولكن يمكن تغييره إذا لزم الأمر.

**Output Example**: 
إذا تم استدعاء الوظيفة بالمدخل التالي:
```python
as_text(b'Hello World')
```
فإن الناتج سيكون:
```python
'Hello World'
```
حيث تم تحويل البيانات الثنائية (`bytes`) إلى نص من نوع `unicode`.
## FunctionDef as_str(bytes_or_text, encoding)
**as_str**: وظيفة `as_str` هي تحويل أي نوع من أنواع البيانات الثنائية (bytes) أو النصوص إلى نص من نوع `str` باستخدام الترميز المحدد.

**parameters**: معاملات هذه الوظيفة.
· `bytes_or_text`: يمكن أن يكون من نوع `bytes`، أو `str`، أو `unicode`. هذا هو المدخل الذي سيتم تحويله إلى نص من نوع `str`.
· `encoding`: سلسلة نصية تحدد ترميز النص المستخدم لفك التشفير. القيمة الافتراضية هي `utf-8`.

**Code Description**: 
تقوم هذه الوظيفة بتحويل أي مدخل نصي أو بيانات ثنائية إلى نص من نوع `str` باستخدام الترميز المحدد. يتم ذلك عن طريق استدعاء وظيفة أخرى تسمى `as_text`، والتي تقوم بتحويل المدخل إلى نص من نوع `unicode` (أو `str` في بايثون 3) باستخدام الترميز المحدد. إذا كان المدخل (`bytes_or_text`) بالفعل من نوع `unicode` أو `str`، يتم إرجاعه كما هو. إذا كان المدخل من نوع `bytes`، يتم فك تشفيره باستخدام الترميز المحدد وإرجاعه كنص من نوع `str`.

في المشروع، يتم استدعاء هذه الوظيفة من خلال وظيفة أخرى تسمى `as_str_any`، والتي تقوم بتحويل أي مدخل إلى نص من نوع `str`. إذا كان المدخل من نوع `bytes`، يتم استدعاء `as_str` لتحويله إلى نص باستخدام الترميز المحدد. إذا كان المدخل من أي نوع آخر، يتم استخدام الدالة `str()` لتحويله إلى نص.

**Note**: 
- يجب التأكد من أن المدخل (`bytes_or_text`) إما من نوع `bytes` أو `unicode`/`str`، وإلا سيتم إطلاق خطأ `TypeError` من خلال الوظيفة `as_text`.
- الترميز الافتراضي هو `utf-8`، ولكن يمكن تغييره إذا لزم الأمر.

**Output Example**: 
إذا تم استدعاء الوظيفة بالمدخل التالي:
```python
as_str(b'Hello World')
```
فإن الناتج سيكون:
```python
'Hello World'
```
حيث تم تحويل البيانات الثنائية (`bytes`) إلى نص من نوع `str` باستخدام الترميز `utf-8`.
## FunctionDef as_str_any(value, encoding)
**as_str_any**: وظيفة `as_str_any` هي تحويل أي مدخل إلى نص من نوع `str`.

**parameters**: معاملات هذه الوظيفة.
· `value`: كائن يمكن تحويله إلى نص من نوع `str`. يمكن أن يكون من أي نوع، مثل `bytes`، أو `str`، أو أي نوع آخر يدعم التحويل إلى نص.
· `encoding`: ترميز النص المستخدم لتحويل البيانات الثنائية (`bytes`) إلى نص. القيمة الافتراضية هي `utf-8`.

**Code Description**: 
تقوم هذه الوظيفة بتحويل أي مدخل إلى نص من نوع `str`. إذا كان المدخل من نوع `bytes`، يتم استدعاء وظيفة أخرى تسمى `as_str` لتحويله إلى نص باستخدام الترميز المحدد (`encoding`). إذا كان المدخل من أي نوع آخر، يتم استخدام الدالة `str()` لتحويله مباشرةً إلى نص.

في المشروع، يتم استدعاء هذه الوظيفة من خلال وظيفة أخرى تسمى `path_to_str`، والتي تقوم بتحويل أي كائن من نوع `PathLike` إلى نص من نوع `str`. إذا كان الكائن يدعم الوظيفة `__fspath__`، يتم استدعاء `as_str_any` لتحويل المسار إلى نص. هذا يضمن أن أي كائن يمثل مسارًا يمكن تحويله إلى نص بسهولة.

**Note**: 
- إذا كان المدخل من نوع `bytes`، يجب التأكد من أن الترميز المحدد (`encoding`) مناسب لفك تشفير البيانات الثنائية.
- إذا كان المدخل من أي نوع آخر، يتم استخدام الدالة `str()` مباشرةً، مما يعني أن أي كائن يدعم التحويل إلى نص يمكن استخدامه مع هذه الوظيفة.

**Output Example**: 
إذا تم استدعاء الوظيفة بالمدخل التالي:
```python
as_str_any(b'Hello World')
```
فإن الناتج سيكون:
```python
'Hello World'
```
حيث تم تحويل البيانات الثنائية (`bytes`) إلى نص من نوع `str` باستخدام الترميز `utf-8`.

إذا تم استدعاء الوظيفة بالمدخل التالي:
```python
as_str_any(123)
```
فإن الناتج سيكون:
```python
'123'
```
حيث تم تحويل الرقم إلى نص من نوع `str` باستخدام الدالة `str()`.
## FunctionDef path_to_str(path)
**path_to_str**: وظيفة `path_to_str` تقوم بتحويل كائن من نوع `PathLike` إلى نص من نوع `str`.

**parameters**: معاملات هذه الوظيفة.
· `path`: كائن يمكن تحويله إلى تمثيل نصي للمسار. يمكن أن يكون من أي نوع يدعم الوظيفة `__fspath__` أو أي نوع آخر.

**Code Description**: 
تقوم هذه الوظيفة بتحويل الكائن المدخل إلى نص من نوع `str` إذا كان الكائن يدعم الوظيفة `__fspath__`. يتم ذلك عن طريق استدعاء الوظيفة `__fspath__` على الكائن المدخل، ثم تحويل الناتج إلى نص باستخدام الوظيفة `as_str_any`. إذا كان الكائن المدخل لا يدعم الوظيفة `__fspath__`، يتم إرجاع الكائن كما هو دون أي تحويل.

العلاقة بين هذه الوظيفة والوظيفة `as_str_any` هي أن `path_to_str` تستخدم `as_str_any` لتحويل الناتج من `__fspath__` إلى نص. هذا يضمن أن أي كائن يمثل مسارًا يمكن تحويله إلى نص بسهولة وبشكل متسق.

**Note**: 
- إذا كان الكائن المدخل لا يدعم الوظيفة `__fspath__`، يتم إرجاع الكائن كما هو دون أي تحويل.
- هذه الوظيفة مفيدة بشكل خاص عند الحاجة إلى تمثيل نصي مبسط للمسار من كائنات `os.PathLike`.

**Output Example**: 
إذا تم استدعاء الوظيفة بالمدخل التالي:
```python
path_to_str(Path('./corpus'))
```
فإن الناتج سيكون:
```python
'corpus'
```
حيث تم تحويل الكائن من نوع `PathLike` إلى نص من نوع `str`.

إذا تم استدعاء الوظيفة بالمدخل التالي:
```python
path_to_str('./.././Corpus')
```
فإن الناتج سيكون:
```python
'./.././Corpus'
```
حيث تم إرجاع المدخل كما هو لأنه لا يدعم الوظيفة `__fspath__`.
## FunctionDef path_to_bytes(path)
**path_to_bytes**: وظيفة `path_to_bytes` تقوم بتحويل كائن من نوع `PathLike` أو `str` إلى نوع `bytes`.

**parameters**: معاملات هذه الوظيفة.
· `path`: كائن يمكن تحويله إلى تمثيل مسار، مثل كائن `PathLike` أو `str`.

**Code Description**: 
تقوم هذه الوظيفة بتحويل المدخل الذي يمثل مسارًا (مثل كائن `PathLike` أو `str`) إلى نوع `bytes`. يتم ذلك عن طريق التحقق مما إذا كان الكائن المدخل يحتوي على طريقة `__fspath__`، وهي طريقة تُستخدم لتحويل الكائن إلى تمثيل نصي للمسار. إذا كان الكائن يحتوي على هذه الطريقة، يتم استدعاؤها للحصول على تمثيل المسار كنص. بعد ذلك، يتم تمرير الناتج إلى وظيفة `as_bytes` لتحويله إلى `bytes`.

العلاقة بين `path_to_bytes` و `as_bytes` هي أن `path_to_bytes` تعتمد على `as_bytes` لإتمام عملية التحويل النهائية إلى `bytes`. بمعنى آخر، `path_to_bytes` تقوم بمعالجة المدخلات الأولية (مثل كائنات `PathLike`) وتحويلها إلى نص، ثم تقوم `as_bytes` بتحويل هذا النص إلى `bytes`. هذه العلاقة تجعل `path_to_bytes` وظيفة مفيدة في الحالات التي تحتاج فيها إلى تمثيل المسار بشكل ثنائي (`bytes`) بدلًا من النصي.

**Note**: 
- يجب أن يكون المدخل (`path`) إما كائنًا يدعم طريقة `__fspath__` أو نصًا (`str`)، وإلا لن تعمل الوظيفة بشكل صحيح.
- الوظيفة تعتمد على `as_bytes` لتحويل النص إلى `bytes`، لذا يجب التأكد من أن المدخل النهائي (بعد التحويل إلى نص) يمكن تحويله إلى `bytes` باستخدام الترميز الافتراضي (`utf-8`) أو أي ترميز آخر محدد.

**Output Example**: 
إذا تم استدعاء الوظيفة بالمدخل التالي:
```python
path_to_bytes("/path/to/file")
```
فإن الناتج سيكون:
```python
b'/path/to/file'
```
وهذا يمثل المسار "/path/to/file" بشكل ثنائي (`bytes`).
</div>
