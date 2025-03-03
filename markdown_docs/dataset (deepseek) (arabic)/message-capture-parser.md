<div dir="rtl">

## ClassDef ProgressBar
**ProgressBar**: وظيفة ProgressBar هي عرض شريط تقدم مرئي يعكس نسبة الإنجاز في عملية معالجة الملفات.

**attributes**: السمات الخاصة بهذا الكلاس.
· total: العدد الإجمالي للوحدات (على سبيل المثال، حجم الملف الكلي) التي يجب معالجتها.
· running: العدد الحالي للوحدات التي تمت معالجتها حتى الآن.

**Code Description**:  
يُستخدم هذا الكلاس لعرض شريط تقدم مرئي في واجهة الأوامر (CLI) أثناء معالجة الملفات. يتم تمرير القيمة الإجمالية (total) عند إنشاء الكائن، والتي تمثل العدد الكلي للوحدات التي يجب معالجتها (مثل حجم الملف الكلي). يحتوي الكلاس على وظيفتين رئيسيتين:

1. **set_progress**: تقوم هذه الوظيفة بحساب النسبة المئوية للإنجاز بناءً على القيمة الحالية (progress) وتحديث شريط التقدم في واجهة الأوامر. يتم حساب عدد الكتل (#) التي يجب عرضها بناءً على عرض الطرفية المتاح (terminal width). إذا كان عرض الطرفية أقل من أو يساوي 12، يتم تجاهل العرض لتجنب تشويه الشكل.

2. **update**: تقوم هذه الوظيفة بتحديث القيمة الحالية (running) بإضافة القيمة الجديدة (more) التي تمثل الوحدات الإضافية التي تمت معالجتها. بعد ذلك، يتم استدعاء set_progress لحساب النسبة المئوية الجديدة وتحديث شريط التقدم.

في المشروع، يتم استخدام ProgressBar في وظيفة process_file لتتبع تقدم قراءة الملفات الثنائية. يتم تحديث شريط التقدم بناءً على عدد البايتات التي تمت قراءتها من الملف. بالإضافة إلى ذلك، يتم استخدام ProgressBar في الوظيفة الرئيسية main لإنشاء شريط تقدم يعكس التقدم الكلي لمعالجة جميع الملفات المحددة.

**Note**:  
- يجب التأكد من أن الطرفية تدعم عرض شريط التقدم (أي أن sys.stdout.isatty() يعيد True).  
- إذا كان عرض الطرفية صغيرًا جدًا (أقل من أو يساوي 12 عمودًا)، لن يتم عرض شريط التقدم لتجنب تشويه الشكل.  
- يتم تحديث شريط التقدم بشكل متكرر أثناء معالجة الملفات، لذا يجب تجنب استخدامه في بيئات غير تفاعلية.

**Output Example**:  
مثال على شكل شريط التقدم عند عرضه في الطرفية:  
`[ #########################                         ]  75%`  
حيث يمثل `#` الجزء المكتمل ويمثل المسافات الجزء المتبقي.
### FunctionDef __init__(self, total)
**__init__**: وظيفة `__init__` هي تهيئة كائن من الفئة `ProgressBar` وتعيين القيم الأولية للخصائص.

**parameters**: معاملات هذه الوظيفة.
· `total`: عدد عشري (float) يمثل القيمة الكلية التي سيتم تتبع التقدم بالنسبة لها.

**Code Description**: 
تقوم هذه الوظيفة بتهيئة كائن من الفئة `ProgressBar` عن طريق تعيين القيمة الممررة للمعامل `total` إلى الخاصية `self.total`. بالإضافة إلى ذلك، يتم تعيين الخاصية `self.running` إلى القيمة `0`، والتي تمثل التقدم الحالي أو القيمة التي تم إنجازها حتى الآن. هذه القيم تُستخدم لاحقًا لتتبع التقدم وإظهاره بشكل مرئي.

**Note**: 
- يجب أن تكون القيمة الممررة للمعامل `total` عددًا عشريًا (float) لضمان عمل الكود بشكل صحيح.
- الخاصية `self.running` يتم تهيئتها دائمًا إلى `0` عند إنشاء الكائن، مما يعني أن التقدم يبدأ من الصفر.
***
### FunctionDef set_progress(self, progress)
**set_progress**: وظيفة `set_progress` هي تحديث شريط التقدم (progress bar) بناءً على النسبة المحددة للتقدم.

**parameters**: معاملات هذه الوظيفة.
· `progress`: قيمة من نوع `float` تمثل النسبة المئوية للتقدم، حيث تكون القيمة بين 0 و 1.

**Code Description**: 
تقوم هذه الوظيفة بتحديث شريط التقدم المعروض في الطرفية بناءً على النسبة المحددة (`progress`). يتم حساب عدد الأعمدة المتاحة في الطرفية باستخدام `shutil.get_terminal_size()`، وإذا كان عدد الأعمدة أقل من أو يساوي 12، فإن الوظيفة لا تقوم بأي شيء لأن العرض غير كافٍ لعرض شريط التقدم. 

بعد ذلك، يتم حساب الحد الأقصى لعدد الكتل (`max_blocks`) التي يمكن عرضها في شريط التقدم عن طريق طرح 9 من عدد الأعمدة المتاحة. يتم بعد ذلك حساب عدد الكتل التي يجب عرضها (`num_blocks`) بناءً على النسبة المحددة (`progress`). 

يتم استخدام الأمر `print` لعرض شريط التقدم في الطرفية، حيث يتم عرض الكتل المملوءة (`#`) والفراغات (` `) بناءً على النسبة المحددة. يتم استخدام `\r` لإعادة المؤشر إلى بداية السطر حتى يتم تحديث شريط التقدم دون طباعة سطر جديد. يتم أيضًا عرض النسبة المئوية للتقدم بجانب شريط التقدم.

في المشروع، يتم استدعاء هذه الوظيفة من خلال وظيفة `update` في الكائن `ProgressBar`، حيث يتم حساب النسبة المئوية للتقدم بناءً على القيمة الجارية (`running`) والقيمة الكلية (`total`)، ثم يتم تمرير هذه النسبة إلى `set_progress` لتحديث شريط التقدم.

**Note**: 
- يجب أن تكون قيمة `progress` بين 0 و 1، حيث تمثل 0 عدم وجود تقدم وتمثل 1 اكتمال التقدم.
- إذا كان عرض الطرفية أقل من أو يساوي 12 عمودًا، فلن يتم عرض شريط التقدم.

**Output Example**: 
عند استدعاء الوظيفة بقيمة `progress` تساوي 0.75، قد يظهر الشريط كما يلي:
```
[ ###########################                         ]  75%
```
حيث تمثل `#` الكتل المملوءة وتمثل المسافات الفراغات المتبقية.
***
### FunctionDef update(self, more)
**update**: وظيفة `update` هي تحديث قيمة التقدم الجارية (`running`) في شريط التقدم بناءً على القيمة المضافة (`more`).

**parameters**: معاملات هذه الوظيفة.
· `more`: قيمة من نوع `float` تمثل الكمية التي سيتم إضافتها إلى قيمة التقدم الجارية (`running`).

**Code Description**: 
تقوم هذه الوظيفة بتحديث قيمة التقدم الجارية (`running`) عن طريق إضافة القيمة المحددة (`more`) إليها. بعد ذلك، يتم حساب النسبة المئوية للتقدم عن طريق قسمة القيمة الجارية (`running`) على القيمة الكلية (`total`)، ثم يتم تمرير هذه النسبة إلى وظيفة `set_progress` لتحديث شريط التقدم المعروض في الطرفية.

في المشروع، يتم استدعاء هذه الوظيفة من خلال وظيفة `process_file` عند قراءة ملف معين. يتم استخدام `update` لتحديث شريط التقدم بناءً على عدد البايتات التي تمت قراءتها من الملف. يتم حساب الفرق بين الموضع الحالي للملف (`f_in.tell()`) والقيمة السابقة (`bytes_read`)، ثم يتم تمرير هذا الفرق إلى `update` لتحديث شريط التقدم.

**Note**: 
- يجب أن تكون القيمة المضافة (`more`) موجبة لضمان تحديث شريط التقدم بشكل صحيح.
- يتم استخدام هذه الوظيفة في سياق قراءة الملفات لتحديث شريط التقدم بشكل تدريجي مع تقدم عملية القراءة.
***
## FunctionDef to_jsonable(obj)
**to_jsonable**: وظيفة to_jsonable هي تحويل كائن معين إلى صيغة قابلة للتحويل إلى JSON.

**parameters**: معاملات هذه الوظيفة.
· obj: الكائن الذي يتم تحويله إلى صيغة قابلة للتحويل إلى JSON. يمكن أن يكون من أي نوع (Any).

**Code Description**: 
تقوم هذه الوظيفة بتحويل الكائن المُدخل (obj) إلى صيغة قابلة للتحويل إلى JSON. تعتمد الوظيفة على نوع الكائن المُدخل لتحديد كيفية التحويل:

1. إذا كان الكائن يحتوي على خاصية `__dict__`، فإن الوظيفة تقوم بإرجاع `__dict__` الخاص بالكائن مباشرةً. هذه الحالة تنطبق على الكائنات التي تحتوي على سمات (attributes) قابلة للوصول عبر `__dict__`.

2. إذا كان الكائن يحتوي على خاصية `__slots__`، فإن الوظيفة تقوم بإنشاء قاموس (dictionary) جديد وتعبئته بقيم السمات الموجودة في `__slots__`. إذا كانت القيمة من النوع `int` وتنتمي إلى `HASH_INTS`، يتم تحويلها إلى سلسلة نصية (hex) باستخدام `ser_uint256`. إذا كانت القيمة عبارة عن قائمة من الأعداد الصحيحة وتنتمي إلى `HASH_INT_VECTORS`، يتم تحويل كل عنصر في القائمة إلى سلسلة نصية (hex). بالنسبة للسمات الأخرى، يتم استدعاء الوظيفة بشكل متكرر لتحويلها.

3. إذا كان الكائن عبارة عن قائمة (list)، يتم تحويل كل عنصر في القائمة باستدعاء الوظيفة بشكل متكرر.

4. إذا كان الكائن من النوع `bytes`، يتم تحويله إلى سلسلة نصية (hex).

5. في الحالات الأخرى، يتم إرجاع الكائن كما هو دون أي تحويل.

في مشروع dataset/message-capture-parser.py، يتم استدعاء هذه الوظيفة من خلال وظيفة `process_file` لتحويل جسم الرسالة (message body) إلى صيغة قابلة للتحويل إلى JSON قبل إضافتها إلى قائمة الرسائل (messages). هذا يساعد في تسهيل عملية تحويل البيانات إلى JSON لاحقًا.

**Note**: 
- يجب أن تكون الكائنات المدخلة إما تحتوي على `__dict__` أو `__slots__` أو تكون من النوع `list` أو `bytes` حتى تعمل الوظيفة بشكل صحيح.
- الوظيفة تعتمد على وجود متغيرات `HASH_INTS` و `HASH_INT_VECTORS` و `ser_uint256` في النطاق (scope) الذي يتم استدعاؤها فيه.

**Output Example**: 
إذا كان الكائن المدخل يحتوي على السمات التالية:
```python
class Example:
    __slots__ = ['a', 'b', 'c']
    def __init__(self):
        self.a = 123
        self.b = [456, 789]
        self.c = b'hello'
```
فإن الناتج سيكون:
```python
{
    'a': '7b',  # تم تحويل العدد 123 إلى سلسلة نصية (hex)
    'b': ['1c8', '315'],  # تم تحويل الأعداد في القائمة إلى سلاسل نصية (hex)
    'c': '68656c6c6f'  # تم تحويل bytes إلى سلسلة نصية (hex)
}
```
## FunctionDef process_file(path, messages, recv, progress_bar)
**process_file**: وظيفة `process_file` هي قراءة ملف ثنائي يحتوي على رسائل، وتحليل هذه الرسائل، وإضافتها إلى قائمة الرسائل (`messages`) في صيغة قابلة للتحويل إلى JSON.

**parameters**: معاملات هذه الوظيفة.
· `path`: مسار الملف الثنائي الذي سيتم قراءته وتحليله. يجب أن يكون من النوع `str`.
· `messages`: قائمة من النوع `List[Any]` يتم إضافة الرسائل المحللة إليها.
· `recv`: قيمة منطقية (`bool`) تحدد ما إذا كانت الرسائل المرسلة أو المستلمة هي التي يتم معالجتها. إذا كانت القيمة `True`، فإن الرسائل تعتبر مستلمة، وإلا فإنها تعتبر مرسلة.
· `progress_bar`: كائن من النوع `Optional[ProgressBar]` يستخدم لتتبع تقدم قراءة الملف. إذا كانت القيمة `None`، لن يتم عرض شريط التقدم.

**Code Description**: 
تقوم هذه الوظيفة بفتح الملف الثنائي المحدد بواسطة `path` وقراءة محتواه بشكل تسلسلي. يتم استخدام `progress_bar` لتحديث شريط التقدم أثناء قراءة الملف، حيث يتم حساب عدد البايتات التي تمت قراءتها وتحديث شريط التقدم وفقًا لذلك.

يتم قراءة رأس كل رسالة (Header) والذي يتكون من ثلاثة أجزاء: الوقت (`time`)، ونوع الرسالة (`msgtype`)، وطول الرسالة (`length`). يتم تحويل هذه القيم إلى أنواع بيانات مناسبة (مثل `int` لـ `time` و`length`، و`bytes` لـ `msgtype`).

بعد ذلك، يتم إنشاء قاموس (`msg_dict`) يحتوي على معلومات الرسالة، بما في ذلك اتجاهها (`direction`)، والوقت (`time`)، والحجم (`size`)، ونوع الرسالة (`msgtype`). يتم تحديد نوع الرسالة باستخدام `MESSAGEMAP`، وهو قاموس يربط بين أنواع الرسائل الثنائية وفئات الرسائل المقابلة.

إذا كان نوع الرسالة غير معروف (غير موجود في `MESSAGEMAP`)، يتم إضافة رسالة تحذير إلى `messages` مع تفاصيل الرسالة غير المعروفة. إذا كان نوع الرسالة معروفًا، يتم تحويل جسم الرسالة إلى صيغة قابلة للتحويل إلى JSON باستخدام وظيفة `to_jsonable` وإضافتها إلى `msg_dict`.

في حالة حدوث خطأ أثناء تحليل الرسالة (مثل عدم القدرة على تحويلها)، يتم إضافة رسالة تحذير إلى `messages` مع تفاصيل الخطأ.

في النهاية، يتم تحديث شريط التقدم ليعكس النهاية الفعلية للملف في حالة الخروج المبكر من الحلقة.

**Note**: 
- يجب أن يكون الملف المحدد بواسطة `path` موجودًا وقابلًا للقراءة.
- يتم استخدام `progress_bar` فقط إذا تم تمرير كائن `ProgressBar` صالح. إذا كان `progress_bar` هو `None`، لن يتم عرض شريط التقدم.
- يتم استدعاء هذه الوظيفة من خلال الوظيفة الرئيسية `main` في المشروع، حيث يتم تمرير قائمة الملفات الثنائية (`capturepaths`) ومعالجتها واحدة تلو الأخرى. يتم استخدام `progress_bar` لتتبع التقدم الكلي لمعالجة جميع الملفات.
- يتم استخدام `to_jsonable` لتحويل جسم الرسالة إلى صيغة قابلة للتحويل إلى JSON قبل إضافتها إلى قائمة الرسائل (`messages`).
## FunctionDef main
**main**: وظيفة `main` هي نقطة الدخول الرئيسية لبرنامج تحليل ملفات التقاط الرسائل الثنائية. تقوم بقراءة ملفات التقاط الرسائل المحددة، وتحليلها، وترتيبها زمنيًا، ثم إخراج النتائج إما إلى ملف أو إلى الطرفية.

**parameters**: لا تحتوي هذه الوظيفة على معاملات مباشرة، ولكنها تستخدم وسائط سطر الأوامر (command-line arguments) التي يتم تمريرها عند تشغيل البرنامج.

**Code Description**:  
تبدأ الوظيفة `main` بإنشاء كائن `ArgumentParser` من مكتبة `argparse` لتسهيل معالجة وسائط سطر الأوامر. يتم تحديد وصف للبرنامج باستخدام `__doc__`، ويتم إضافة مثال توضيحي لكيفية استخدام البرنامج في نهاية الرسالة المساعدة (epilog).  

يتم تعريف ثلاثة وسائط رئيسية:  
1. `capturepaths`: مسارات ملفات التقاط الرسائل الثنائية التي سيتم تحليلها. يمكن تحديد أكثر من ملف باستخدام `nargs='+'`.  
2. `--output` أو `-o`: مسار الملف الذي سيتم حفظ النتائج فيه. إذا لم يتم تحديده، يتم إخراج النتائج إلى الطرفية (stdout).  
3. `--no-progress-bar` أو `-n`: خيار لتعطيل شريط التقدم. يتم تعطيله تلقائيًا إذا لم يكن الإخراج إلى طرفية تفاعلية.  

بعد تحليل الوسائط، يتم تحويل مسارات الملفات إلى كائنات `Path` لتسهيل التعامل معها. يتم أيضًا تحديد ما إذا كان سيتم استخدام شريط التقدم بناءً على وجود الخيار `--no-progress-bar` وما إذا كان الإخراج إلى طرفية تفاعلية.  

يتم إنشاء قائمة `messages` لتخزين الرسائل المحللة. إذا تم تفعيل شريط التقدم، يتم حساب الحجم الكلي لجميع الملفات المحددة وإنشاء كائن `ProgressBar` لتتبع التقدم.  

يتم بعد ذلك معالجة كل ملف باستخدام الوظيفة `process_file`، حيث يتم تحليل الرسائل وإضافتها إلى قائمة `messages`. يتم تحديث شريط التقدم أثناء معالجة كل ملف.  

بعد الانتهاء من معالجة جميع الملفات، يتم ترتيب الرسائل زمنيًا باستخدام المفتاح `time`. إذا كان شريط التقدم مفعلًا، يتم تحديثه ليعكس اكتمال العملية.  

أخيرًا، يتم تحويل قائمة الرسائل إلى صيغة JSON باستخدام `json.dumps`. إذا تم تحديد ملف إخراج، يتم كتابة النتائج إليه. وإلا، يتم طباعة النتائج إلى الطرفية.  

**Note**:  
- يجب أن تكون ملفات التقاط الرسائل المحددة موجودة وقابلة للقراءة.  
- إذا تم تعطيل شريط التقدم أو إذا لم يكن الإخراج إلى طرفية تفاعلية، لن يتم عرض شريط التقدم.  
- يتم استخدام الوظيفة `process_file` لتحليل كل ملف على حدة، ويتم استخدام `ProgressBar` لتتبع التقدم الكلي لمعالجة جميع الملفات.  
- يتم ترتيب الرسائل زمنيًا قبل إخراجها لضمان تسلسل زمني صحيح.
</div>
