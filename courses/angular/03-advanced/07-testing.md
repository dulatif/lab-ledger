# Lesson 7: Unit Testing

## Learning Goals

- TestBed setup.
- Testing Components.

## 1. Default Setup (Jasmine/Karma)

Traditionally Angular uses Karma (browser runner) and Jasmine (assertion).
Modern Angular is moving towards **Jest** or **Web Test Runner**.

## 2. TestBed

The Angular testing module emulator.

```typescript
describe("UserProfileComponent", () => {
  let component: UserProfileComponent;
  let fixture: ComponentFixture<UserProfileComponent>;

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      imports: [UserProfileComponent], // Import standalone
      providers: [provideHttpClient()], // Mock/Provide deps
    }).compileComponents();

    fixture = TestBed.createComponent(UserProfileComponent);
    component = fixture.componentInstance;
    fixture.detectChanges();
  });

  it("should create", () => {
    expect(component).toBeTruthy();
  });

  it("should render title", () => {
    const compiled = fixture.nativeElement as HTMLElement;
    expect(compiled.querySelector("h1")?.textContent).toContain("Profile");
  });
});
```

## Key Takeaways

- **TestBed** is powerful but slow.
- Mock your services (especially HTTP) using `jasmine.createSpyObj`.
