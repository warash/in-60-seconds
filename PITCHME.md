# Angular 2 performance pitfalls

---
#### ChangeDetectionStrategy OnPush
@snap[west span-50]
Change detection mechanism
@snapend
@snap[east span-50]
![IMAGE](assets/onpush-cd.gif)
@snapend
---
#### OnPush - problems with third party libs 
@ul[list-spaced-bullets]
- its risky to use OnPush on close to top level with deep children tree
- you must to return to Angular Zone when using library from outside angular ecosystem
@ulend
---
#### OnPush - problems subscription to shared state

@ul[list-spaced-bullets]
- use | async pipes
- use changeDetectionRef.markForCheck
@ulend
---

#### NgZone - Move frequent event that are not changing angular state outside ngZone
@ul[list-spaced-bullets]
- do not listen to scroll, mouse and other haevy events inside angular unless its necessary
- do not attach listeneres to global evenets like domcument.click or xhr.onReadyStateChange
@ulend
---

#### TrackBy *NgFor
use it whenever you are using immutables. NgFor compares list items by reference. If refernce changes it recreates entire item from scratch.
---


#### use ShareReplay(1) and DistinctUntilChanged with observables
@ul[list-spaced-bullets]
- always use shareReplay(1) on streams you wanna make public and use with async inside your view
- to prevent view model computation if no needed select from golbal state only dependant properties and add distinctUntiChanged(isEqual)
@ulend
---


#### avoid binding to methods or getters
@ul[list-spaced-bullets]
- caclulate your viewmodel only when necessary 
- use memoize from lodash
- recalc model when dependand inputs changed inside ngOnChanges method
- using methods or getters for binding causes recalculation every changedetection cycle
- methods or getters returns always new instance which causes children with onPush to rerender 
@ulend
---

