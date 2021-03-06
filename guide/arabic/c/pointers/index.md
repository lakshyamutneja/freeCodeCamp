---
title: Pointers
localeTitle: مؤشرات
---
# المؤشرات في C

الآن يجب أن تكون على علم بأن لغة C هي لغة منخفضة المستوى ، ولا شيء يدل على ذلك أفضل من المؤشرات. المؤشرات هي المتغيرات التي تحصل على قيمة المتغير "بالإشارة" إلى موقع ذاكرة بدلاً من تخزين قيمة المتغير نفسه. وهذا يسمح ببعض الحيل المفيدة ، وهو ما يمنحنا أيضًا الوصول إلى المصفوفات ومعالجة الملفات ، من بين أمور أخرى.

#

 `type *var-name; 
` 

## صنع واستخدام مؤشر

 `#include <stdio.h> 
 
 int main(void){ 
    double my_double_variable = 10.1; 
    double *my_pointer; 
 
    my_pointer = &my_double_variable; 
 
    printf("value of my_double_variable: %f\n", my_double_variable); 
 
    ++my_double_variable; 
 
    printf("value of my_pointer: %f\n", *my_pointer); 
 
    return 0; 
 } 
` 

انتاج:

 `value of my_double_variable: 10.100000 
 value of my_pointer: 11.100000 
` 

في هذا الرمز ، هناك إعلانان. الأول هو تهيئة متغيرة نموذجية مما يؤدي إلى `double` وتعيينها تساوي 10.1. الجديد في تصريحاتنا هو استخدام `*` . تستخدم العلامة النجمية ( `*` ) عادةً للضرب ، ولكن عندما نستخدمها بوضعها أمام متغير ، فإنها تخبر C بأن هذا متغير مؤشر.

يخبر السطر التالي المحول البرمجي حيث يوجد ذلك في مكان آخر. باستخدام `&` بهذه الطريقة ، يصبح "مشغل إلغاء الإشارة" ، ويعيد موقع الذاكرة للمتغير الذي ينظر إليه.

مع أخذ ذلك في الاعتبار ، دعونا نلقي نظرة أخرى على هذا الرمز من التعليمات البرمجية:

 `double *my_pointer; 
 // my_pointer now stored the address of my_double_variable 
 my_pointer = &my_double_variable; 
` 

تم الإعلان عن `my_pointer` وتم الإعلان عنه كمؤشر. يعرف المترجم C الآن أن `my_pointer` إلى موقع ذاكرة. يعيّن السطر التالي `my_pointer` قيمة موقع ذاكرة باستخدام `&` .

الآن دعونا نلقي نظرة على ما يشير إلى الإشارة إلى موقع الذاكرة لرمزك:

 `    printf("value of my_double_variable: %f\n", my_double_variable); 
 
    // Same as my_double_variable = my_double_variable + 1 
    // In human language, adding one to my_double_variable 
    ++my_double_variable; 
 
    printf("value of my_pointer: %f\n", *my_pointer); 
` 

لاحظ أنه للحصول على قيمة البيانات الموجودة في `*my_pointer` ، ستحتاج إلى إخبار C أنك تريد الحصول على القيمة التي يشير إليها المتغير. حاول تشغيل هذا الكود بدون هذه العلامة النجمية ، وستتمكن من طباعة موقع الذاكرة ، لأن هذا هو ما `my_variable` المتغير `my_variable` بالفعل.

يمكنك تعريف مؤشر متعدد في عبارة واحدة كما هو الحال مع المتغيرات القياسية ، مثل:

 `int *x, *y; 
` 

لاحظ أن `*` مطلوب قبل كل متغير. وهذا لأن كونك مؤشرًا يعتبر جزءًا من المتغير وليس جزءًا من نوع البيانات.

## الاستخدامات العملية للمؤشرات

### المصفوفات

التطبيق الأكثر شيوعًا لمؤشر موجود في صفيف. الصفائف ، التي ستقرأ عنها لاحقًا ، تسمح لمجموعة من المتغيرات. لا يتعين عليك فعليًا التعامل مع `*` و `&` لاستخدام المصفوفات ، ولكن هذا ما يفعلونه وراء الكواليس.

### المهام

في بعض الأحيان تريد ضبط قيمة متغير داخل دالة ، ولكن إذا قمت ببساطة بالمرور في القيمة ذات القيمة المتغيرة ، ستعمل الدالة مع نسخة من المتغير الخاص بك بدلاً من المتغير نفسه. إذا قمت بدلاً من ذلك بتمرير المؤشر في الإشارة إلى موقع الذاكرة للمتغير ، فيمكنك الوصول إليه وتعديله من خارج نطاقه الطبيعي. هذا لأنك تلمس موقع الذاكرة الأصلي نفسه ، مما يسمح لك بضبط شيء ما في إحدى الوظائف وإجراء تغييرات في مكان آخر. على النقيض من "call by value" ، يسمى هذا "call by reference".

يقوم البرنامج التالي بتبديل قيم متغيرين داخل دالة `swap` المخصصة. لتحقيق ذلك ، يتم تمرير المتغيرات بالإشارة.

 ` /* C Program to swap two numbers using pointers and function. */ 
 #include <stdio.h> 
 void swap(int *n1, int *n2); 
 
 int main() 
 { 
    int num1 = 5, num2 = 10; 
 
    // address of num1 and num2 is passed to the swap function 
    swap( &num1, &num2); 
    printf("Number1 = %d\n", num1); 
    printf("Number2 = %d", num2); 
    return 0; 
 } 
 
 void swap(int * n1, int * n2) 
 { 
    // pointer n1 and n2 points to the address of num1 and num2 respectively 
    int temp; 
    temp = *n1; 
    *n1 = *n2; 
    *n2 = temp; 
 } 
` 

انتاج |

 `Number1 = 10 
 Number2 = 5 
` 

يتم تمرير العناوين ، أو مواقع الذاكرة ، من `num1` و `num2` إلى `swap` الدالة ويتم تمثيلها بواسطة المؤشرات `*n1` و `*n2` داخل الدالة. لذلك ، الآن تشير المؤشرات `n1` و `n2` إلى عناوين `num1` و `num2` على التوالي.

لذا ، يشير الآن المؤشر n1 و n2 إلى عنوان num1 و num2 على التوالي.

عندما يتم تغيير قيمة المؤشرات ، تتغير القيمة في موقع الذاكرة المدورة أيضًا.

وبالتالي ، تنعكس التغييرات التي تم إجراؤها على \* n1 و \* n2 في num1 و num2 في الوظيفة الرئيسية.

### المؤشرات كمواصفات إلى وظيفة

عندما نمرر أي معلمة لتعمل ، فإننا نقوم بعمل نسخة من المعلمة. دعونا نرى مع المثال

 `#include <stdio.h> 
 
 void func(int); 
 
 int main(void) { 
    int a = 11; 
    func(a); 
    printf("%d",a);// print 11 
 
 
    return 0; 
 } 
 void func(int a){ 
 a=5 
 printf("%d",a);//print 5 
 } 
` 

في المثال أعلاه ، نقوم بتغيير قيمة الرقم الصحيح في الدالة func ، لكننا لا نزال نحصل على 11 في الوظيفة الرئيسية. يحدث هذا لأنه في وظيفة نسخة من عدد صحيح مرت كمعلمة ، لذلك في هذه الوظيفة لا يمكننا الوصول إلى "a" في الوظيفة الرئيسية.

إذن كيف يمكنك تغيير قيمة العدد الصحيح المحدد بشكل أساسي ، باستخدام وظيفة أخرى؟ هنا تأتي POINTERS في الدور. عندما نوفر المؤشر كمعلمة ، يمكننا الوصول إلى عنوان هذه المعلمة ويمكننا الوصول إلى أي من هذه المعلمات مع النتيجة وسيتم عرض النتيجة في كل مكان. أدناه مثال يوضح بالضبط نفس الشيء الذي نريده ...

من خلال `n1` الإشارة إلى `n1` و `n2` ، يمكننا الآن تعديل الذاكرة التي تشير إلى نقطة `n1` و `n2` . هذا يسمح لنا بتغيير قيمة المتغيرين `num1` و `num2` المعلنين في الوظيفة `main` خارج `num2` الطبيعي. بعد الانتهاء من الوظيفة ، قام المتغيران الآن بتبديل قيمهما ، كما يمكن رؤيتها في الإخراج.

### الحيل مع مواقع الذاكرة

كلما كان من الممكن تجنب ذلك ، من المستحسن إبقاء التعليمات البرمجية الخاصة بك سهلة القراءة والفهم. في أفضل السيناريوهات ، فإن شفرتك ستحكي القصة - سيكون من السهل قراءة أسماء المتغيرات ، ومن المنطقي إذا قرأتها بصوت عالٍ ، وستستخدم التعليق أحيانًا لتوضيح ما يفعله سطر الشفرة.

ولهذا السبب ، يجب توخي الحذر عند استخدام المؤشرات. من السهل إنشاء شيء مربك بالنسبة لك لتصحيح الأخطاء أو قراءة شخص آخر. ومع ذلك ، فمن الممكن القيام ببعض الأشياء الجميلة معهم.

ألقِ نظرة على هذا الرمز ، الذي يحول شيء من الأحرف الكبيرة إلى الصغيرة:

 `#include <stdio.h> 
 #include <ctype.h> 
 
 char *lowerCase (char *string) { 
    char *p = string; 
    while (*p) { 
        if (isupper(*p)) *p = tolower(*p); 
        p++; 
    } 
    return string; 
 } 
` 

يبدأ هذا عن طريق اتخاذ سلسلة (شيء ستتعرف عليه عندما تصل إلى المصفوفات) ويمر عبر كل موقع. لاحظ في p ++. هذا يزيد المؤشر ، مما يعني أنه يبحث في موقع الذاكرة التالي. كل حرف هو موقع ذاكرة ، لذلك في هذه الحالة المؤشر يتطلع إلى كل حرف واتخاذ قرار بشأن ما يجب القيام به لكل واحد.

### كونست Qualifer

يمكن تطبيق الثوابت المؤهلة على تعريف أي متغير لتحديد أنه لن يتم تغيير قيمته (والذي يعتمد على حيث يتم تخزين المتغيرات const ، قد نقوم بتغيير قيمة متغير const باستخدام المؤشر).

# مؤشر للمتغير

يمكننا تغيير قيمة ptr ويمكننا أيضًا تغيير قيمة ptr object التي تشير إلى. يشرح جزء التعليمات البرمجية التالي مؤشر إلى متغير

 `#include <stdio.h> 
 int main(void) 
 { 
    int i = 10; 
    int j = 20; 
    int *ptr = &i;        /* pointer to integer */ 
    printf("*ptr: %d\n", *ptr); 
 
    /* pointer is pointing to another variable */ 
    ptr = &j; 
    printf("*ptr: %d\n", *ptr); 
 
    /* we can change value stored by pointer */ 
    *ptr = 100; 
    printf("*ptr: %d\n", *ptr); 
 
    return 0; 
 } 
` 

# مؤشر إلى ثابت

يمكننا تغيير المؤشر للإشارة إلى أي متغير صحيح آخر ، ولكن لا يمكن تغيير قيمة الكائن (الكيان) المشار إليه باستخدام مؤشر ptr.

 `#include <stdio.h> 
 int main(void) 
 { 
    int i = 10; 
    int j = 20; 
    const int *ptr = &i;    /* ptr is pointer to constant */ 
 
    printf("ptr: %d\n", *ptr); 
    *ptr = 100;        /* error: object pointed cannot be modified 
                     using the pointer ptr */ 
 
    ptr = &j;          /* valid */ 
    printf("ptr: %d\n", *ptr); 
 
    return 0; 
 } 
` 

# مؤشر ثابت للمتغير

في هذا يمكننا تغيير قيمة المتغير الذي يشير إليه المؤشر. ولكن لا يمكننا تغيير المؤشر للإشارة إليه متغير آخر.

 `#include <stdio.h> 
 int main(void) 
 { 
   int i = 10; 
   int j = 20; 
   int *const ptr = &i;    /* constant pointer to integer */ 
 
   printf("ptr: %d\n", *ptr); 
 
   *ptr = 100;    /* valid */ 
   printf("ptr: %d\n", *ptr); 
 
   ptr = &j;        /* error */ 
   return 0; 
 } 
` 

# مؤشر ثابت لثابت

الإعلان أعلاه هو مؤشر ثابت للمتغير الثابت مما يعني أنه لا يمكننا تغيير القيمة التي يشير إليها المؤشر وكذلك لا يمكننا توجيه المؤشر إلى متغير آخر.

 `#include <stdio.h> 
 
 int main(void) 
 { 
    int i = 10; 
    int j = 20; 
    const int *const ptr = &i; /* constant pointer to constant integer */ 
 
    printf("ptr: %d\n", *ptr); 
 
    ptr = &j;            /* error */ 
    *ptr = 100;        /* error */ 
 
    return 0; 
 } 
` 

# قبل أن تذهب ...

## مراجعة

*   المؤشرات هي متغيرات ، ولكن بدلاً من تخزين قيمة ، فإنها تخزن موقع ذاكرة.
*   `*` و `&` تستخدم للوصول إلى القيم في مواقع الذاكرة والوصول إلى مواقع الذاكرة ، على التوالي.
*   المؤشرات مفيدة لبعض الميزات الأساسية لـ C.

# مؤشر مقابل صفيف في C

في أغلب الأحيان ، يمكن التعامل مع الوصول إلى المؤشر والمصفوفة على أنها تعمل بنفس الطريقة ، أما الاستثناءات الرئيسية فهي:

1) مشغل sizeof

*   إرجاع `sizeof(array)` مقدار الذاكرة المستخدمة من قبل كافة العناصر في الصفيف
*   يسترد `sizeof(pointer)` فقط مقدار الذاكرة المستخدمة من قبل متغير المؤشر نفسه

2) و المشغل

*   & array هو اسم مستعار لصفيف \[0\] وإرجاع عنوان العنصر الأول في الصفيف
*   ويعيد المؤشر عنوان المؤشر

3) تهيئة سلسلة حرفية لصفيف الحروف

*   يعيّن `char array[] = “abc”` العناصر الأربعة الأولى في الصفيف إلى "a" و "b" و "c" و "\\ 0"
*   `char *pointer = “abc”` يحدد المؤشر إلى عنوان السلسلة "abc" (التي يمكن تخزينها في ذاكرة القراءة فقط وبالتالي غير قابلة للتغيير)

4) يمكن تعيين متغير مؤشر قيمة بينما لا يمكن أن يكون متغير الصفيف.

 `    int a[10]; 
    int *p; 
    p = a; /*legal*/ 
    a = p; /*illegal*/ 
` 

5) يسمح الحساب على متغير المؤشر.

 `    p++; /*Legal*/ 
    a++; /*illegal*/ 
`