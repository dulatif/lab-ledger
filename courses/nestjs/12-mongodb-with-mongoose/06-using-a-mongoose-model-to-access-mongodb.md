# 6. Using a Mongoose Model to Access MongoDB

## 🎯 Learning Goal

Register the model and inject it into a Service.

## 🧠 Concept

You need to tell NestJS "Hey, I want to use the Coffee model in this module".

## 💻 Implementation

### 1. Register in Feature Module

```typescript
imports: [
  MongooseModule.forFeature([{ name: Coffee.name, schema: CoffeeSchema }]),
];
```

### 2. Inject in Service

```typescript
import { Model } from "mongoose";
import { InjectModel } from "@nestjs/mongoose";

@Injectable()
export class CoffeesService {
  constructor(
    @InjectModel(Coffee.name) private readonly coffeeModel: Model<Coffee>
  ) {}

  findAll() {
    return this.coffeeModel.find().exec();
  }

  create(createCoffeeDto: CreateCoffeeDto) {
    const coffee = new this.coffeeModel(createCoffeeDto);
    return coffee.save();
  }
}
```

## 🧩 Activity / Challenge

1.  Complete the CRUD (`findOne`, `update`, `remove`).
2.  Use Postman to Create a coffee.
3.  Check Compass. You see a `coffees` collection.

## 🔑 Key Takeaways

- Always use `.exec()` at the end of finding queries in Mongoose 6+ (returns a real Promise).
- `new this.coffeeModel(data)` creates the instance locally. `.save()` persists it.
