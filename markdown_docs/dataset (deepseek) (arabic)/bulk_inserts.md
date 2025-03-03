<div dir="rtl">

## ClassDef Customer
**Customer**: وظيفة الفئة Customer هي تمثيل جدول قاعدة بيانات يحتوي على بيانات العملاء.  
**attributes**: السمات الخاصة بهذه الفئة.  
· `id`: عمود من نوع Integer يمثل المفتاح الأساسي للجدول، ويتم إنشاؤه تلقائيًا باستخدام Identity().  
· `name`: عمود من نوع String بطول 255 حرفًا، يمثل اسم العميل.  
· `description`: عمود من نوع String بطول 255 حرفًا، يمثل وصفًا للعميل.  

**Code Description**:  
تمثل الفئة Customer جدولًا في قاعدة البيانات يُسمى "customer"، ويحتوي على ثلاثة أعمدة رئيسية:  
1. `id`: وهو المفتاح الأساسي للجدول، ويتم إنشاؤه تلقائيًا باستخدام Identity()، مما يضمن أن كل عميل يحصل على معرف فريد.  
2. `name`: يُستخدم لتخزين اسم العميل، وهو عبارة عن نص بطول أقصى يصل إلى 255 حرفًا.  
3. `description`: يُستخدم لتخزين وصف إضافي للعميل، وهو أيضًا نص بطول أقصى يصل إلى 255 حرفًا.  

في المشروع، يتم استخدام هذه الفئة في عدة اختبارات لإدراج بيانات العملاء بطرق مختلفة:  
- في `test_flush_no_pk`، يتم إدراج عملاء بدون تحديد المفتاح الأساسي (`id`)، حيث يتم إنشاؤه تلقائيًا.  
- في `test_flush_pk_given`، يتم إدراج عملاء مع تحديد المفتاح الأساسي (`id`) مسبقًا.  
- في `test_orm_bulk_insert`، يتم إدراج عملاء باستخدام ORM بطريقة "bulk" دون استرجاع الصفوف المدرجة.  
- في `test_orm_insert_returning`، يتم إدراج عملاء باستخدام ORM مع استرجاع الكائنات المدرجة.  
- في `test_core_insert`، يتم إدراج عملاء باستخدام Core API بطريقة bulk.  
- في `test_dbapi_raw`، يتم إدراج عملاء باستخدام DBAPI مباشرة.  

تظهر هذه الاختبارات كيفية استخدام الفئة Customer في إدراج البيانات بطرق مختلفة، سواء باستخدام ORM أو Core API أو حتى DBAPI مباشرة.  

**Note**:  
- عند استخدام الفئة Customer، تأكد من أن طول النص في `name` و `description` لا يتجاوز 255 حرفًا.  
- إذا كنت تستخدم ORM، يمكنك الاستفادة من الميزات التلقائية مثل إنشاء `id` دون الحاجة إلى تحديده يدويًا.  
- في حالة استخدام Core API أو DBAPI، يجب التأكد من توفير البيانات بشكل صحيح بما يتوافق مع هيكل الجدول.
## FunctionDef setup_database(dburl, echo, num)
**setup_database**: وظيفة `setup_database` هي إعداد قاعدة البيانات عن طريق إنشاء محرك قاعدة البيانات وحذف الجداول الحالية وإنشاءها من جديد.

**parameters**: معاملات هذه الوظيفة.
· `dburl`: عنوان URL الخاص بقاعدة البيانات الذي يتم استخدامه للاتصال بقاعدة البيانات.
· `echo`: معامل منطقي (boolean) يحدد ما إذا كان سيتم عرض استعلامات SQL في السجل (log) أم لا. إذا كان `True`، سيتم عرض الاستعلامات، وإذا كان `False`، فلن يتم عرضها.
· `num`: معامل غير مستخدم حاليًا في الكود المقدم، وقد يكون مخصصًا لاستخدامات مستقبلية.

**Code Description**: 
تقوم الوظيفة `setup_database` بإعداد قاعدة البيانات بالخطوات التالية:
1. يتم إنشاء محرك قاعدة البيانات (`engine`) باستخدام الدالة `create_engine` من مكتبة SQLAlchemy. يتم تمرير عنوان URL الخاص بقاعدة البيانات (`dburl`) ومعامل `echo` الذي يتحكم في عرض استعلامات SQL.
2. يتم حذف جميع الجداول الحالية في قاعدة البيانات باستخدام `Base.metadata.drop_all(engine)`. هذه الخطوة تضمن أن قاعدة البيانات تبدأ من حالة نظيفة بدون أي جداول موجودة مسبقًا.
3. يتم إنشاء جميع الجداول من جديد باستخدام `Base.metadata.create_all(engine)`. هذه الجداول يتم تعريفها في الكلاس `Base` الذي يتم استيراده من مكان آخر في المشروع.

**Note**: 
- تأكد من أن الكلاس `Base` والمتغير `engine` تم تعريفهما بشكل صحيح في مكان آخر في المشروع.
- استخدام `Base.metadata.drop_all(engine)` سيحذف جميع الجداول الموجودة، لذا يجب استخدام هذه الوظيفة بحذر في بيئات الإنتاج حيث قد يؤدي ذلك إلى فقدان البيانات.
- المعامل `num` غير مستخدم حاليًا، لذا يمكن تجاهله أو إزالته إذا لم يكن هناك حاجة مستقبلية له.
## FunctionDef test_flush_no_pk(n)
**test_flush_no_pk**: وظيفة `test_flush_no_pk` هي اختبار إدراج بيانات العملاء في قاعدة البيانات باستخدام ORM دون تحديد المفتاح الأساسي (`id`) بشكل صريح، مع استخدام `flush` لتنفيذ الإدراج على دفعات واسترجاع معرف الصف المولد تلقائيًا.

**parameters**: معاملات هذه الوظيفة.  
· `n`: عدد العملاء المراد إدراجهم في قاعدة البيانات.  

**Code Description**:  
تقوم هذه الوظيفة باختبار إدراج بيانات العملاء في قاعدة البيانات باستخدام ORM (Object-Relational Mapping) دون الحاجة إلى تحديد المفتاح الأساسي (`id`) بشكل يدوي. يتم ذلك من خلال الخطوات التالية:  

1. يتم إنشاء جلسة (`session`) للتعامل مع قاعدة البيانات باستخدام محرك (`engine`) محدد.  
2. يتم تقسيم عدد العملاء (`n`) إلى دفعات (batches) بحجم 1000 عميل لكل دفعة.  
3. لكل دفعة، يتم إنشاء قائمة من كائنات `Customer` تحتوي على بيانات العملاء (الاسم والوصف) باستخدام حلقة `for`.  
4. يتم إضافة هذه الكائنات إلى الجلسة باستخدام `session.add_all`.  
5. يتم تنفيذ عملية `flush` للجلسة، مما يؤدي إلى إدراج البيانات في قاعدة البيانات واسترجاع معرف الصف (`id`) المولد تلقائيًا إذا كان متاحًا.  
6. بعد إدراج جميع الدفعات، يتم تنفيذ `commit` لحفظ التغييرات بشكل دائم في قاعدة البيانات.  

تستخدم هذه الوظيفة فئة `Customer` لتمثيل جدول العملاء في قاعدة البيانات. يتم إنشاء كائنات `Customer` مع تحديد `name` و`description` فقط، بينما يتم توليد `id` تلقائيًا بواسطة قاعدة البيانات.  

**Note**:  
- تأكد من أن قيمة `n` كبيرة بما يكفي لاختبار إدراج عدد كبير من العملاء.  
- يتم تقسيم الإدراج إلى دفعات لتحسين الأداء وتجنب مشاكل الذاكرة عند التعامل مع أعداد كبيرة من البيانات.  
- إذا كانت قاعدة البيانات تدعم `RETURNING`، فسيتم استرجاع معرف الصف (`id`) المولد تلقائيًا بعد كل عملية `flush`.  
- هذه الوظيفة مفيدة لاختبار أداء إدراج البيانات باستخدام ORM دون الحاجة إلى تحديد المفتاح الأساسي يدويًا.
## FunctionDef test_flush_pk_given(n)
**test_flush_pk_given**: وظيفة `test_flush_pk_given` هي إدراج بيانات العملاء في قاعدة البيانات باستخدام ORM مع تحديد المفتاح الأساسي (`id`) مسبقًا.

**parameters**: معاملات هذه الوظيفة.  
· `n`: عدد العملاء المراد إدراجهم في قاعدة البيانات.  

**Code Description**:  
تقوم هذه الوظيفة بإدراج بيانات العملاء في قاعدة البيانات باستخدام ORM (Object-Relational Mapping) مع تحديد المفتاح الأساسي (`id`) مسبقًا. يتم تنفيذ الإدراج على دفعات (batches) بحجم 1000 عميل في كل دفعة.  

1. يتم إنشاء جلسة (session) مع قاعدة البيانات باستخدام محرك (engine) محدد.  
2. يتم تقسيم العدد الكلي للعملاء (`n`) إلى دفعات بحجم 1000 عميل.  
3. لكل دفعة، يتم إنشاء قائمة من كائنات `Customer` مع تحديد المفتاح الأساسي (`id`) واسم العميل (`name`) ووصف العميل (`description`) لكل عميل.  
4. يتم إضافة هذه القائمة إلى الجلسة باستخدام `session.add_all`.  
5. يتم تنفيذ `session.flush` بعد كل دفعة لإرسال البيانات إلى قاعدة البيانات دون إغلاق الجلسة.  
6. بعد إدراج جميع الدفعات، يتم تنفيذ `session.commit` لحفظ التغييرات بشكل دائم في قاعدة البيانات.  

تستخدم هذه الوظيفة فئة `Customer` لتمثيل بيانات العملاء. يتم تحديد المفتاح الأساسي (`id`) يدويًا لكل عميل، مما يسمح بالتحكم الكامل في قيم المفاتيح الأساسية.  

**Note**:  
- تأكد من أن قيمة `n` الممررة إلى الوظيفة أكبر من أو تساوي 1.  
- يتم تقسيم العملاء إلى دفعات بحجم 1000 لتحسين الأداء وتجنب مشاكل الذاكرة.  
- يجب أن تكون قيم `id` المحددة فريدة لتجنب تعارضات المفاتيح الأساسية في قاعدة البيانات.  
- يمكن تعديل حجم الدفعة (1000) حسب احتياجات الأداء وقدرات النظام.
## FunctionDef test_orm_bulk_insert(n)
Doc is waiting to be generated...
## FunctionDef test_orm_insert_returning(n)
Doc is waiting to be generated...
## FunctionDef test_core_insert(n)
**test_core_insert**: وظيفة `test_core_insert` هي إدراج بيانات عملاء بشكل جماعي (bulk) باستخدام Core API.  

**parameters**: معاملات هذه الوظيفة.  
· `n`: عدد صحيح يحدد عدد الصفوف التي سيتم إدراجها في جدول العملاء.  

**Code Description**:  
تقوم هذه الوظيفة بإدراج بيانات عملاء بشكل جماعي في جدول قاعدة البيانات باستخدام Core API. يتم ذلك من خلال تنفيذ عملية إدراج واحدة (INSERT) لعدد `n` من الصفوف في جدول `customer`.  

1. يتم فتح اتصال بقاعدة البيانات باستخدام `engine.begin()`، مما يضمن أن العملية تتم ضمن نطاق transaction واحد.  
2. يتم تنفيذ عملية الإدراج باستخدام `conn.execute()`، حيث يتم تمرير كائن `Customer.__table__.insert()` كأول معامل، والذي يمثل عملية إدراج في جدول `customer`.  
3. يتم تمرير قائمة من القواميس كمعامل ثانٍ، حيث يحتوي كل قاموس على بيانات عميل واحد. يتم إنشاء هذه القائمة باستخدام list comprehension، حيث يتم إنشاء `n` عميلًا بأسماء وأوصاف فريدة بناءً على الفهرس `i`.  
4. يتم إغلاق الاتصال تلقائيًا بعد انتهاء العملية بسبب استخدام `with` statement.  

تظهر هذه الوظيفة كيفية استخدام Core API لإدراج بيانات بشكل جماعي دون الحاجة إلى استخدام ORM. وهي تعتمد على فئة `Customer` التي تمثل جدول `customer` في قاعدة البيانات، حيث يتم إدراج البيانات في الأعمدة `name` و `description`.  

**Note**:  
- تأكد من أن قيمة `n` الممررة إلى الوظيفة هي عدد صحيح موجب.  
- عند استخدام Core API، يجب أن تكون البيانات الممررة متوافقة مع هيكل الجدول المحدد في فئة `Customer`.  
- هذه الوظيفة مفيدة في اختبارات الأداء أو عند الحاجة إلى إدراج عدد كبير من الصفوف بشكل فعال.
## FunctionDef test_dbapi_raw(n)
**test_dbapi_raw**: وظيفة `test_dbapi_raw` هي إدراج صفوف في قاعدة البيانات بشكل جماعي باستخدام واجهة برمجة التطبيقات الخاصة بقاعدة البيانات (DBAPI).

**parameters**: معاملات هذه الوظيفة.
· `n`: عدد الصفوف التي سيتم إدراجها في جدول العملاء.

**Code Description**: 
تقوم هذه الوظيفة بإدراج عدد محدد من الصفوف في جدول العملاء (`customer`) باستخدام واجهة برمجة التطبيقات الخاصة بقاعدة البيانات (DBAPI) بشكل مباشر. يتم ذلك من خلال الخطوات التالية:

1. يتم إنشاء اتصال بقاعدة البيانات باستخدام `engine.pool._creator()`، والذي يعيد كائن اتصال (`conn`).
2. يتم إنشاء مؤشر (`cursor`) من خلال الاتصال لتنفيذ الاستعلامات.
3. يتم تجميع استعلام الإدراج (`INSERT`) باستخدام جدول `Customer`، حيث يتم تحديد القيم التي سيتم إدراجها (`name` و `description`) باستخدام `bindparam`.
4. يتم تحديد نوع المعاملات (positional أو named) بناءً على ما إذا كان الاستعلام المترجم يستخدم المعاملات الموضعية أم لا. إذا كان يستخدم المعاملات الموضعية، يتم إنشاء قائمة من القيم كمجموعة من tuples. وإلا، يتم إنشاء قائمة من القيم كمجموعة من dictionaries.
5. يتم تنفيذ الاستعلام باستخدام `cursor.executemany()`، حيث يتم تمرير الاستعلام المترجم والقيم التي سيتم إدراجها.
6. يتم تأكيد التغييرات في قاعدة البيانات باستخدام `conn.commit()`.
7. يتم إغلاق الاتصال بقاعدة البيانات باستخدام `conn.close()`.

تستخدم هذه الوظيفة جدول `Customer` الذي تم تعريفه في المشروع، حيث يحتوي الجدول على الأعمدة `id` (المفتاح الأساسي)، و`name` (اسم العميل)، و`description` (وصف العميل). يتم إدراج البيانات بشكل مباشر دون استخدام ORM أو Core API، مما يجعل هذه الوظيفة مثالًا على كيفية التعامل مع قاعدة البيانات باستخدام DBAPI مباشرة.

**Note**: 
- تأكد من أن القيم التي يتم إدراجها في الأعمدة `name` و `description` لا تتجاوز 255 حرفًا، كما هو محدد في تعريف الجدول.
- هذه الوظيفة مفيدة لاختبار أداء إدراج البيانات بشكل جماعي باستخدام DBAPI مباشرة، ولكن في التطبيقات العملية، يُفضل عادةً استخدام ORM أو Core API لسهولة الصيانة والأمان.
</div>
