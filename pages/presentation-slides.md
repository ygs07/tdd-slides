---
title: Test Driven Development (TDD)
subtitle: Red-Green-Yellow Cycle in Laravel + Pest + Vitest
paginate: true
---

# Test Driven Development (TDD)

🚀 Red-Green-Yellow Cycle in Laravel + Pest + Vitest

Tests are essentially scenarios for your app to pass through without breaking.

TDD is an approach to coding that prioritises creating these scenraios during development.

- [Slide deck 📚 here](https://ygs07.github.io/tdd-slides)

---

# How would you describe a Perfect Application?

<div v-click>

No Bugs?

</div>
<div v-click>

Working Apis?

</div>
<div v-click>

A Perfect UI?

</div>
<div v-click>

### A Perfect Application I'd say is the one with minimal features so it's very stable.

<hr/>

</div>

<div class="mt-5" v-click>
- And it's less likely to break. Like the Calculator app. But even that breaks sometimes.
</div>

<div class="mt-5" v-click>
- But as your user-base grows, more and more feature requests come in due to the needs of user groups. And as a result your app will start exiting that comfort zone of <span class="font-bold"> Stability </span>.
</div>

<v-click>

What's one way You Maintain Stability?
</v-click>

---

# Enter TDD?

<v-click>

Write code guided by **tests**
</v-click>

<v-click>

Cycle: Write test → Code → Refactor

</v-click>

<v-click>

Helps avoid bugs early
</v-click>

<v-click>

Promotes clean & maintainable code
</v-click>

<v-click>

Future proofs your application, ensuring that new feature will not break existing ones
</v-click>

---

# Testing Libraries: Pest And Vitest

🧪 Pest (PHP)

- Simple & expressive syntax for PHP Unit Tests

- Built on top of PHPUnit

- Supports Unit, Feature, E2E (via Laravel Dusk)

- Snapshot testing, parallel testing, etc.

- Requires PHP 8.2

- Very simple syntax

[PEST Documentation](https://pestphp.com/docs/installation)

```php
// php Example
it('works', function () {
expect(true)->toBeTrue();
});
```

---

# 🧪 Vitest (JS/TS)

- Fast unit/integration testing for JavaScript/TypeScript

- Built-in support for mocks, spies, snapshots

- Vite-powered → fast startup & HMR-like experience

- Integrates with Cypress for E2E

- Requires npm v18

- Comes with a testing UI

[Vitest Documentation](https://vitest.dev/guide/ui.html)

```ts
test("works", () => {
  expect(true).toBe(true);
});
```

---

# The Red-Green-Yellow Cycle

<v-click>
🚦 Red → Write a failing test  
</v-click>

<v-click>

✅ Green → Make it pass  
</v-click>

<v-click>

🟡 Yellow → Refactor for better design  
</v-click>

<v-click>

🔁 Repeat!
</v-click>

---

# 🚦 Red Stage: Write a Failing Test

<v-click>

-Define **what** you want to build

I want to be able to get the full names of my users by concatinating their first_name, middle_name and last_name

</v-click>

<v-click>

-Write a **failing** test first

```php
//PHP example, with PEST or with PHP
it('returns a user\'s full name', function () {
    $user = new User(['first_name' => 'John', 'last_name' => 'Doe']);
    expect($user->fullName())->toBe('John Doe'); // 🔴 Fails! Method doesn't exist yet.
});
```

```ts
// Vitest Example
test("returns a user's full name", () => {
  const user = { firstName: "John", lastName: "Doe", fullName: () => "" };
  expect(user.fullName()).toBe("John Doe"); // 🔴 Fails!
});
```

</v-click>

---

# ✅ Green Stage: Make It Pass

- Write **just enough** code to pass the test

<v-click>

```php
//App\Models\User
class User {
    public function fullName() {
        return "{$this->first_name} {$this->last_name}";
    }
}
```

```ts
// Vitest Example
const user = {
  firstName: "John",
  lastName: "Doe",
  fullName() {
    return `${this.firstName} ${this.lastName}`;
  },
};

expect(user.fullName()).toBe("John Doe");
```

</v-click>

---

# 🟡 Yellow Stage: Refactor

<v-click>
- Clean up the code without breaking tests
</v-click>

<v-click>
```php
//App\Models\User
class User {
    public function fullName(): string {
        return trim("{$this->first_name} {$this->last_name}");
    }
}
```

```ts
// Vitest Example
const user = {
  firstName: "John",
  lastName: "Doe",
  fullName() {
    return `${this.firstName.trim()} ${this.lastName.trim()}`;
  },
};

expect(user.fullName()).toBe("John Doe");
```

</v-click>

---

# 🧪 Types of Tests

- **Unit Tests AKA Integration Tests**
- **Feature Tests**
- **End-to-End (E2E) Tests**

---

# ⚙️ Unit Tests

- Test **smallest** pieces of code in **isolation**
- No database or external calls

```php
it('calculates discount correctly', function () {
    $product = new Product(['price' => 100]);
    expect($product->discountPrice(10))->toBe(90);
});
```

```ts
// Vitest Example
test("calculates discount correctly", () => {
  const product = {
    price: 100,
    discountPrice: (percent) => product.price - (product.price * percent) / 100,
  };
  expect(product.discountPrice(10)).toBe(90);
});
```

---

# 🌐 Feature Tests

- Test **features** of the app
- Use HTTP routes, database, etc.

```php
it('shows the user dashboard', function () {
    $user = User::factory()->create();

    $this->actingAs($user)
         ->get('/dashboard')
         ->assertStatus(200);
});
```

```ts
// Vitest Example (using supertest + express)
import request from "supertest";
import app from "../src/app";

test("shows the user dashboard", async () => {
  const res = await request(app)
    .get("/dashboard")
    .set("Authorization", "Bearer mock-token");

  expect(res.status).toBe(200);
});
```

---

# 🧭 End-to-End (E2E) Tests

- Test **entire user flows**
- Browser simulation (Laravel Dusk, Cypress)

```php
use Laravel\Dusk\Browser;

$this->browse(function (Browser $browser) {
    $browser->visit('/login')
            ->type('email', 'john@example.com')
            ->type('password', 'secret')
            ->press('Login')
            ->assertPathIs('/dashboard');
});
```

```ts
// Cypress Example
describe("Login Flow", () => {
  it("logs user in and redirects to dashboard", () => {
    cy.visit("/login");
    cy.get("input[name=email]").type("john@example.com");
    cy.get("input[name=password]").type("secret");
    cy.contains("Login").click();
    cy.url().should("include", "/dashboard");
  });
});
```

---

# Practical Example:

Lets Jump into some code, I've got some repos setup locally but feel free to:

[Click here to visit the github Repo for the Laravel App here](https://sli.dev/features/line-highlighting)

[Click here to visit the github Repo for the Vitest App here](https://github.com/ygs07/tdd-ws)

---

# Key Takeaways

- TDD improves code quality & confidence
- Red-Green-Yellow keeps you focused
- Use the right test for the right job:
  - Unit: Logic
  - Feature: Integration
  - E2E: User Flow
- When your fundamental tests are put in place writing code that breaks an application becomes difficult
- Using Githooks, tests can be made to run on every push request or a merge request, making it possible to prevent code that fail tests to reach your repo.

<v-click>

<span v-mark.circle.orange="2"> A large scale Application can not Stand without test. </span>

</v-click>

---

## Let's Talk TDD In the Context of SchoolTry

<img height="150" width="190" class="inline-block" src="https://schooltry.com/images/logo.png" alt=""/>

---

# Questions?

🙏 Ask away!

- [Slide deck 📚 here](https://ygs07.github.io/tdd-slides)
