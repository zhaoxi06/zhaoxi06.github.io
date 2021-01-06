---
title: 防止Core模块被重复导入
date: 2020-04-07 15:30:42
tags: angular
---
Core模块只会在一开始式被导入到根模块中，不允许被其他模块重复导入。

<!--more-->

```
import { NgModule, Optional, SkipSelf } from '@angular/core';
import { CommonModule } from '@angular/common';

@NgModule({
    imports: [CommonModule],
})
export class CoreModule {
    constructor (@Optional() @SkipSelf() parentModule: CoreModule) {
        //如果Core模块已经被导入过了，即parentModule不为空时，抛出错误，防止重复导入
        if (parentMoudle) {
            throw new Error(
                'CoreModule is alrelady loaded. Import it in the AppModule only'
            );
        }
    }
}
```