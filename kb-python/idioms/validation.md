# Python Validation

**Language:** Python · **Category:** Data Modeling

Strategy: kb-core/patterns/data-modeling. Here: Python mechanics.

**What:** Pydantic v2 models = parse + validate + coerce. Boundary in, typed out.

```python
class CreateUser(BaseModel):
    email: EmailStr
    age: int = Field(ge=0, le=130)

u = CreateUser.model_validate(body)   # raises ValidationError
```

**Gotchas:** v2 coerces by default; `model_config = ConfigDict(extra="forbid")` rejects extras; dataclasses don't validate; EmailStr needs `email-validator`.

**See also:** kb-core data-modeling; kb-go/rust/typescript.
