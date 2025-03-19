# Imported Slides

You can split your slides.md into multiple files and organize them as you want using the `src` attribute.

#### `slides.md`

```markdown
# Page 1

Page 2 from main entry.

---

## src: ./subpage.md
```

<br>

#### `subpage.md`

```markdown
# Page 2

Page 2 from another file.
```

```php
uses(Tests\TestCase::class);

it('ensures APP_DEBUG is false in production', function () {
    Config::set('app.env', 'production');
    Config::set('app.debug', false);

    expect(Config::get('app.env'))->toBe('production');
    expect(Config::get('app.debug'))->toBeFalse();
});
```

```ts
// Vitest Example
test("ensures APP_DEBUG is false in production", () => {
  const env = "production";
  const debug = false;

  expect(env).toBe("production");
  expect(debug).toBe(false);
});
```

[Learn more](https://sli.dev/guide/syntax.html#importing-slides)
