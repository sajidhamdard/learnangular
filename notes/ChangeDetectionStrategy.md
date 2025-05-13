# 🧩 **What is ChangeDetectionStrategy?**

Angular uses **Change Detection** to check if something has changed in the app (inputs, variables…) and update the DOM.

By default:
Angular **checks everything** (even if not needed)

👉 This is called
`ChangeDetectionStrategy.Default` (the default)

We can **optimize performance** by telling Angular:
“Hey! Check **only if inputs change** — not unnecessarily”

👉 This is called
`ChangeDetectionStrategy.OnPush`

---

# ⚙️ **How to use it?**

```ts
@Component({
  selector: 'app-child',
  template: `{{name}}`,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class ChildComponent {
  @Input() name!: string;
}
```

---

# 🥇 **What is difference?**

| Strategy    | When Angular runs change detection                                                |
| ----------- | --------------------------------------------------------------------------------- |
| **Default** | Every time parent changes or any event happens anywhere                           |
| **OnPush**  | Only when **@Input() reference** changes or component triggers detection manually |

---

# 🔥 **Practical Example**

Let’s say:
We have **ParentComponent** and **ChildComponent**

## ParentComponent

```ts
@Component({
  template: `
    <app-child [name]="user.name"></app-child>
    <button (click)="changeName()">Change Name</button>
    <button (click)="increment()">Increment Counter</button>
    {{counter}}
  `
})
export class ParentComponent {
  user = { name: 'John' };
  counter = 0;

  changeName() {
    this.user = { name: 'Jane' };   // New object (triggers OnPush)
  }

  increment() {
    this.counter++;
  }
}
```

## ChildComponent

```ts
@Component({
  selector: 'app-child',
  template: `Child Name: {{name}}`,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class ChildComponent {
  @Input() name!: string;
}
```

---

# ✅ **What happens?**

* `increment()` triggers re-render of **Parent**
* **With Default strategy** → Child is also checked unnecessarily
* **With OnPush** → Child is **not checked** (unless `user` reference changes)

When `changeName()` is called → new `user` object triggers Child update

---

# 🥇 **When to use OnPush?**

| Use case                                                                         | Why use OnPush?              |
| -------------------------------------------------------------------------------- | ---------------------------- |
| Components with mostly @Input() data and no internal state                       | Avoid unnecessary checks     |
| Large lists (like tables, cards, etc.)                                           | Boost performance            |
| Reusable dumb/presentational components                                          | Safer & faster               |
| Apps using **immutable data** (like `user = {...}` instead of `user.name = ...`) | Works best with immutability |

---

# 💡 **How to confidently explain in interview**

> *"By default, Angular runs change detection everywhere on every event. With OnPush, Angular only runs change detection if an input reference changes or I explicitly trigger it. This improves performance by avoiding unnecessary DOM updates, especially in large lists or reusable components."*

---

# ⚡ **Pro Tips (to impress)**

| Question interviewer asks                        | Killer Answer                                                                                  |
| ------------------------------------------------ | ---------------------------------------------------------------------------------------------- |
| How do you manually trigger detection in OnPush? | Use `ChangeDetectorRef.markForCheck()` or `detectChanges()`                                    |
| What if I mutate object properties directly?     | Won't work. Always change object reference to trigger OnPush (`user = {...user, name: 'New'}`) |
| Can OnPush work with Observables?                | Yes — use **`async` pipe**, it updates view automatically                                      |

---

# 🏆 **Final takeaway**

> **"OnPush = Performance + Predictability (with immutable data)"**

---

# 🧐 **If OnPush is so good, why isn’t it default?**

Because **OnPush comes with responsibility**
It **assumes**:

* You know how change detection works
* You manage **immutability** carefully
* You avoid mutating objects/arrays directly

> In short: **Performance boost** at the cost of **developer discipline**

---

# 🥇 **Key Reasons why OnPush is not default**

| Reason                                                                | Explanation                                                                                                               |
| --------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| 1️⃣ Most devs don’t manage immutability                               | OnPush works well **only** if we change object/array references (not mutate directly) — many devs forget this             |
| 2️⃣ Complex apps (forms, stateful UIs) need frequent internal changes | In such cases, Default strategy (automatic detection) is safer and less error-prone                                       |
| 3️⃣ Backward compatibility                                            | Angular can't suddenly make OnPush default — it would break existing apps                                                 |
| 4️⃣ Less experienced teams                                            | Teams unfamiliar with immutability patterns (like Redux, NGRX) would face bugs/confusion                                  |
| 5️⃣ Manual work sometimes                                             | You may need to call `markForCheck()` explicitly if data is updated outside Angular zone (setTimeout, external API, etc.) |

---

# ⚡ **When should you confidently choose OnPush?**

| Scenario                                               | OnPush Recommended?          |
| ------------------------------------------------------ | ---------------------------- |
| Pure Presentational / Dumb Components                  | ✅ Yes                        |
| API data shown via Observable + async                  | ✅ Yes                        |
| Large lists / tables / grids                           | ✅ Yes                        |
| Components with minimal logic/state                    | ✅ Yes                        |
| Complex interactive forms with dynamic controls        | ❌ Stick to Default (simpler) |
| Component where you frequently mutate objects directly | ❌ Stick to Default           |

---

# 🥇 **How to answer confidently in interview?**

> *"OnPush is a great performance tool — but it assumes disciplined immutability and knowledge of Angular’s internals. That’s why Angular keeps Default as safe fallback. I personally prefer OnPush for reusable presentational components or API-driven components, but for complex stateful forms or legacy code, Default is safer and easier."*

---

# 🏆 **Final takeaway**

✅ **OnPush = Scalpel** → Precise, powerful, but requires skill
✅ **Default = Swiss Knife** → Safer, works everywhere without extra care

---


# 🧩 **Scenario**

We have a `UserService` that fetches users (returns Observable).
We want to show user list in **ChildComponent (with OnPush)**.

---

# 1️⃣ **UserService** (fake API)

```ts
@Injectable({ providedIn: 'root' })
export class UserService {
  getUsers(): Observable<string[]> {
    return of(['John', 'Jane', 'Alice', 'Bob']).pipe(delay(1000));
  }
}
```

---

# 2️⃣ **UserListComponent (OnPush + async pipe)**

```ts
@Component({
  selector: 'app-user-list',
  template: `
    <h3>User List</h3>
    <ul>
      <li *ngFor="let user of users$ | async">
        {{user}}
      </li>
    </ul>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class UserListComponent {
  @Input() users$!: Observable<string[]>;
}
```

🟢 **Key points**
✅ We use `async` pipe → It **automatically triggers change detection** when Observable emits
✅ No need to manually call `markForCheck()`
✅ OnPush works **perfectly fine with async pipe**

---

# 3️⃣ **ParentComponent**

```ts
@Component({
  selector: 'app-parent',
  template: `
    <app-user-list [users$]="users$"></app-user-list>
  `
})
export class ParentComponent {
  users$ = this.userService.getUsers();

  constructor(private userService: UserService) {}
}
```

---

# ✅ **What happens?**

* When component loads → **users\$ Observable emits list**
* `async` pipe triggers update
* No unnecessary change detection cycles
* **Very performant + clean**

---

# 🥇 **How to explain confidently in interview**

> *"OnPush works seamlessly with Observables if we use async pipe. The async pipe subscribes, handles emissions, and automatically triggers change detection for us — no need for manual calls. This combination of OnPush + async pipe is perfect for data-driven UIs that fetch from API or store."*

---

# ⚡ **Bonus Pro Tip (to impress)**

If interviewer asks:
*"What if I want to refresh users on button click?"*

You can **reassign users\$** (immutability friendly)

```ts
refresh() {
  this.users$ = this.userService.getUsers();
}
```

> Because Observable **reference changes**, **OnPush** view updates correctly 💯

---

# 🏆 **Final takeaway**

✅ OnPush + async pipe = **Ideal combo** for API data, store data, realtime data
✅ Eliminates manual subscription/unsubscription headache
✅ Fast, clean, scalable UI

---
