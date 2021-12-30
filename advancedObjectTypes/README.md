## Interfaces and Types

TypeScript allows you to specifically type an object using an interface that can be reused by multiple objects. To create an interface, use the `interface` keyword followed by the interface name and the typed object.

```tsx
interface Mail {
  postagePrice: number;
  address: string;
}

const catalog: Mail = ...
```

The difference between `interface` and `type` is that interface can only be used to type objects, while `type` can be used to type objects, primitives, and more. As it turns out, `type` is more versatile and functional than `interface`. However, we don’t want a type that can do everything sometimes. We’d like our types to be constrained so we’re more likely to write consistent code.Since `interface` may only type objects, it’s a perfect match for writing object-oriented programs because these programs need many typed objects.
