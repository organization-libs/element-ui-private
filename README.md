# 基于Element-ui2.7.2进行个性化变更

## 修改的内容

~~~ text
  -根据自有业务需求主要修改了级联下拉列表的部分交互功能
~~~

## 涉及代码
~~~ js
  // packages/cascader/src/main.vue
  // 原代码
  // <el-input
  //     ref="input"
  //     :readonly="readonly"   
  //     :placeholder="currentLabels.length ? undefined : placeholder"
  //     v-model="inputValue"
  //     @input="debouncedInputChange"
  //     @focus="handleFocus"
  //     @blur="handleBlur"
  //     @compositionstart.native="handleComposition"
  //     @compositionend.native="handleComposition"
  //     :validate-event="false"
  //     :size="size"
  //     :disabled="cascaderDisabled"
  //     :class="{ 'is-focus': menuVisible }"
  //   >

  // 修改后

  // <el-input
  //     ref="input"
  //     :placeholder="currentLabels.length ? undefined : placeholder"
  //     v-model="inputValue"
  //     @input="debouncedInputChange"
  //     @focus="handleFocus"
  //     @blur="handleBlur"
  //     @compositionstart.native="handleComposition"
  //     @compositionend.native="handleComposition"
  //     :validate-event="false"
  //     :size="size"
  //     :disabled="cascaderDisabled"
  //     :class="{ 'is-focus': menuVisible }"
  //   >

  // 修改前
  // <i
  //   key="2"
  //   v-else
  //   class="el-input__icon el-icon-arrow-down"
  //   :class="{ 'is-reverse': menuVisible }"
  // ></i>

  //修改后
  // <i
  //   key="2"
  //   v-else
  //   class="el-input__icon el-icon-arrow-down"
  //   style="position:relative;z-index:100000"
  //   :class="{ 'is-reverse': menuVisible }"
  //   @click="toggleMenu"
  // ></i>
  
  // methods 中加入如下方法
  // toggleMenu(e) {
  //  if (this.disabled) return;// 开启禁用
  //   this.menuVisible = !this.menuVisible;
  //   console.log(this.menuVisible, 'this.menuVisible');
  //   window.event ? window.event.cancelBubble = true : e.stopPropagation();
  // },
  // AddhandleInputChange(value) {
  //   if (value === '') { this.menuVisible = false; return;};
  //   if (!this.menuVisible) this.menuVisible = true;
  //   this.$nextTick(() => {
  //     console.log(value);
  //     this.handleInputChange(value);
  //   });
  //   // this.handleInputChange(value);
  // },

  //created 钩子函数修改为如下
  // created() {
  //   this.debouncedInputChange = debounce(this.debounce, value => {
  //     const before = this.beforeFilter(value);

  //     if (before && before.then) {
  //       this.menu.options = [{
  //         __IS__FLAT__OPTIONS: true,
  //         label: this.t('el.cascader.loading'),
  //         value: '',
  //         disabled: true
  //       }];
  //       before
  //         .then(() => {
  //           this.$nextTick(() => {
  //             this.AddhandleInputChange(value);
  //           });
  //         });
  //     } else if (before !== false) {
  //       this.$nextTick(() => {
  //         this.AddhandleInputChange(value);
  //       });
  //     }
  //   });
  // },

  // handleInputChange方法修改为如下

  // handleInputChange(value) {
  //   console.log(value);
  //   // if (!this.menuVisible) return;
  //   const flatOptions = this.flatOptions;

  //   if (!value) {
  //     this.menu.options = this.options;
  //     this.$nextTick(this.updatePopper);
  //     return;
  //   }

  //   let filteredFlatOptions = flatOptions.filter(optionsStack => {
  //     return optionsStack.some(option => new RegExp(escapeRegexpString(value), 'i')
  //       .test(option[this.labelKey]));
  //   });

  //   if (filteredFlatOptions.length > 0) {
  //     filteredFlatOptions = filteredFlatOptions.map(optionStack => {
  //       return {
  //         __IS__FLAT__OPTIONS: true,
  //         value: optionStack.map(item => item[this.valueKey]),
  //         label: this.renderFilteredOptionLabel(value, optionStack),
  //         disabled: optionStack.some(item => item[this.disabledKey])
  //       };
  //     });
  //   } else {
  //     filteredFlatOptions = [{
  //       __IS__FLAT__OPTIONS: true,
  //       label: this.t('el.cascader.noMatch'),
  //       value: '',
  //       disabled: true
  //     }];
  //   }
  //   this.menu.options = filteredFlatOptions;
  //   this.$nextTick(this.updatePopper);
  // },
  
  // handleClick 方法修改为如下
  // handleClick() {
  //   console.log('in44');
  //   if (this.cascaderDisabled) return;
  //   this.$refs.input.focus();
  //   if (this.inputValue === '') return;
  //   if (this.filterable) {
  //     this.menuVisible = true;
  //     return;
  //   }
  //   this.menuVisible = !this.menuVisible;
  // },
```

## github存放地址

``` js
  // https://github.com/organization-libs/element-ui-private
```