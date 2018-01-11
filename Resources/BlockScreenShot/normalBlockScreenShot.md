### normalBlockScreenShot

```
        void (^testBlock)(void) = ^{
            NSLog(@"test");
        };
        testBlock();
        return 0;
        
```

```
96615 struct __main_block_impl_0 {
96616   struct __block_impl impl;
96617   struct __main_block_desc_0* Desc;
96618   __main_block_impl_0(void *fp, struct __main_block_desc_0 *desc, int flags=0) {
96619     impl.isa = &_NSConcreteStackBlock;
96620     impl.Flags = flags;
96621     impl.FuncPtr = fp;
96622     Desc = desc;
96623   }
96624 };
96625 static void __main_block_func_0(struct __main_block_impl_0 *__cself) {
96626 
96627             NSLog((NSString *)&__NSConstantStringImpl__var_folders_2k_hnly35r943b_l031dscpp1300000gn_T_main_0655e3_mi_0);
96628         }
96629 
96630 static struct __main_block_desc_0 {
96631   size_t reserved;
96632   size_t Block_size;
96633 } __main_block_desc_0_DATA = { 0, sizeof(struct __main_block_impl_0)};
96634 
96635 int main(int argc, char * argv[]) {
96636     /* @autoreleasepool */ { __AtAutoreleasePool __autoreleasepool;
96637         
96638         void (*testBlock)(void) = ((void (*)())&__main_block_impl_0((void *)__main_block_func_0,                                  &__main_block_desc_0_DATA));
96639         
96640         ((void (*)(__block_impl *))((__block_impl *)testBlock)->FuncPtr)((__block_impl *)testBlock);
96641         return 0;
96642     }
96643 }


```


### instanceBlockScreenShot

```
NSArray *array = @[];
        void (^testBlock)(void) = ^{
            NSLog(@"%zd",array.count);
        };
        testBlock();

```

```
struct __main_block_impl_0 {
96616   struct __block_impl impl;
96617   struct __main_block_desc_0* Desc;
96618   NSArray *array;
96619   __main_block_impl_0(void *fp, struct __main_block_desc_0 *desc, NSArray *_array, int flags=0) : array(_array) {
96620     impl.isa = &_NSConcreteStackBlock;
96621     impl.Flags = flags;
96622     impl.FuncPtr = fp;
96623     Desc = desc;
96624   }
96625 };
96626 static void __main_block_func_0(struct __main_block_impl_0 *__cself) {
96627   NSArray *array = __cself->array; // bound by copy
96628 
96629             NSLog((NSString *)&__NSConstantStringImpl__var_folders_2k_hnly35r943b_l031dscpp1300000gn_T_main_1048a5_mi_0,          ((NSUInteger (*)(id, SEL))(void *)objc_msgSend)((id)array, sel_registerName("count")));
96630         }
96631 static void __main_block_copy_0(struct __main_block_impl_0*dst, struct __main_block_impl_0*src) {_Block_object_assign((void*      )&dst->array, (void*)src->array, 3/*BLOCK_FIELD_IS_OBJECT*/);}
96632 
96633 static void __main_block_dispose_0(struct __main_block_impl_0*src) {_Block_object_dispose((void*)src->array, 3/*                  BLOCK_FIELD_IS_OBJECT*/);}
96634 
96635 static struct __main_block_desc_0 {
96636   size_t reserved;
96637   size_t Block_size;
96638   void (*copy)(struct __main_block_impl_0*, struct __main_block_impl_0*);
96639   void (*dispose)(struct __main_block_impl_0*);
96640 } __main_block_desc_0_DATA = { 0, sizeof(struct __main_block_impl_0), __main_block_copy_0, __main_block_dispose_0};

      int main(int argc, char * argv[]) {
96642     /* @autoreleasepool */ { __AtAutoreleasePool __autoreleasepool;
96643 
96644         NSArray *array = ((NSArray *(*)(Class, SEL, ObjectType  _Nonnull const * _Nonnull, NSUInteger))(void *                    )objc_msgSend)(objc_getClass("NSArray"), sel_registerName("arrayWithObjects:count:"), (const id *)__NSContainer_literal(0U).      arr, 0U);
96645         void (*testBlock)(void) = ((void (*)())&__main_block_impl_0((void *)__main_block_func_0, &__main_block_desc_0_DATA,       array, 570425344));
96646         ((void (*)(__block_impl *))((__block_impl *)testBlock)->FuncPtr)((__block_impl *)testBlock);
96647         return 0;
96648     }
96649 }


```

### __blockBlockScreenShot

```

__block NSArray *array = @[];
        void (^testBlock)(void) = ^{
            array = [NSArray new];
        };
        testBlock();

```

```
strauct __Block_byref_array_0 {
96620   void *__isa;
96621 __Block_byref_array_0 *__forwarding;
96622  int __flags;
96623  int __size;
96624  void (*__Block_byref_id_object_copy)(void*, void*);
96625  void (*__Block_byref_id_object_dispose)(void*);
96626  NSArray *array;
96627 };
96628 
96629 struct __main_block_impl_0 {
96630   struct __block_impl impl;
96631   struct __main_block_desc_0* Desc;
96632   __Block_byref_array_0 *array; // by ref
96633   __main_block_impl_0(void *fp, struct __main_block_desc_0 *desc, __Block_byref_array_0 *_array, int flags=0) :                   array(_array->__forwarding) {
96634     impl.isa = &_NSConcreteStackBlock;
96635     impl.Flags = flags;
96636     impl.FuncPtr = fp;
96637     Desc = desc;
96638   }
96639 };
96640 static void __main_block_func_0(struct __main_block_impl_0 *__cself) {
96641   __Block_byref_array_0 *array = __cself->array; // bound by ref
96642 
96643             (array->__forwarding->array) = ((NSArray *(*)(id, SEL))(void *)objc_msgSend)((id)objc_getClass("NSArray"),            sel_registerName("new"));
96644         }
96645 static void __main_block_copy_0(struct __main_block_impl_0*dst, struct __main_block_impl_0*src) {_Block_object_assign((void*      )&dst->array, (void*)src->array, 8/*BLOCK_FIELD_IS_BYREF*/);}

static void __main_block_dispose_0(struct __main_block_impl_0*src) {_Block_object_dispose((void*)src->array, 8/*                  BLOCK_FIELD_IS_BYREF*/);}
96648 
96649 static struct __main_block_desc_0 {
96650   size_t reserved;
96651   size_t Block_size;
96652   void (*copy)(struct __main_block_impl_0*, struct __main_block_impl_0*);
96653   void (*dispose)(struct __main_block_impl_0*);
96654 } __main_block_desc_0_DATA = { 0, sizeof(struct __main_block_impl_0), __main_block_copy_0, __main_block_dispose_0};
96655 int main(int argc, char * argv[]) {
96656     /* @autoreleasepool */ { __AtAutoreleasePool __autoreleasepool;
96657 
96658         __attribute__((__blocks__(byref))) __Block_byref_array_0 array = {(void*)0,(__Block_byref_array_0 *)&array,               33554432, sizeof(__Block_byref_array_0), __Block_byref_id_object_copy_131, __Block_byref_id_object_dispose_131, ((NSArray *       (*)(Class, SEL, ObjectType  _Nonnull const * _Nonnull, NSUInteger))(void *)objc_msgSend)(objc_getClass("NSArray"),                sel_registerName("arrayWithObjects:count:"), (const id *)__NSContainer_literal(0U).arr, 0U)};
96659         void (*testBlock)(void) = ((void (*)())&__main_block_impl_0((void *)__main_block_func_0, &__main_block_desc_0_DATA,       (__Block_byref_array_0 *)&array, 570425344));
96660         ((void (*)(__block_impl *))((__block_impl *)testBlock)->FuncPtr)((__block_impl *)testBlock);
96661         return 0;
96662     }
96663 }


```

### __weakBlockScreenShot

```
NSArray *array = @[];
        __weak typeof(array) _weakArray = array;
        void (^testBlock)(void) = ^{
            NSLog(@"%zd",_weakArray.count);
        };
        testBlock();

```



