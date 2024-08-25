`BaseModel.model_validate(dict) => model_object`
``BaseModel..model_validate_json(json_str) => model_object`

`model_object.model_dump() => dict
`model_object.model_dump_json() => json` (types will be case to primitives (`.value` for enums / str for dates))

## Using Fields for Customisation and Metadata

```python
class Employee(BaseModel):
    employee_id: UUID = Field(default_factory=uuid4, frozen=True)
    name: str = Field(min_length=1, frozen=True)
    email: EmailStr = Field(pattern=r".+@example\.com$")
    date_of_birth: date = Field(alias="birth_date", repr=False, frozen=True)
    salary: float = Field(alias="compensation", gt=0, repr=False)
    department: Department
    elected_benefits: bool
```

- **`default_factory`**: define a callable that generates default values. In the example above, you set `default_factory` to `uuid4`. This calls `uuid4()` to generate a random UUID for `employee_id` when needed. You can also use a [lambda](https://realpython.com/python-lambda/) function for more flexibility.
- **`frozen`**: This is a Boolean parameter you can set to make your fields immutable. This means, when `frozen` is set to `True`, the corresponding field can’t be changed after your model is instantiated.
- **`alias`**: You can use this parameter when you want to assign an alias to your fields. For example, you can allow `date_of_birth` to be called `birth_date` or `salary` to be called `compensation`.
- **`repr`**: This Boolean parameter determines whether a field is displayed in the model’s field representation. In this example, you won’t see `date_of_birth` or `salary` when you print an `Employee` instance.

## Working With Validators

> Field validators must be defined **a class methods**

```python
    @field_validator("date_of_birth")
    @classmethod
    def check_valid_age(cls, date_of_birth: date) -> date:
        today = date.today()
        eighteen_years_ago = date(today.year - 18, today.month, today.day)

        if date_of_birth > eighteen_years_ago:
            raise ValueError("Employees must be at least 18 years old.")

        return date_of_birth
```

> Use `@model_validator` to write checks for multiple fields

```python
    @model_validator(mode="after")
    def check_it_benefits(self) -> Self:
        department = self.department
        elected_benefits = self.elected_benefits

        if department == Department.IT and elected_benefits:
            raise ValueError(
                "IT employees are contractors and don't qualify for benefits"
            )
        return self
```

When you set `mode` to `after` in `@model_validator`, Pydantic waits until after you’ve instantiated your model to run `.check_it_benefits()`.

## Using Validation Decorators to Validate Functions

```python
@validate_call
def send_invoice(
    client_name: Annotated[str, Field(min_length=1)],
    client_email: EmailStr,
    items_purchased: list[str],
    amount_owed: PositiveFloat,
) -> str:
```

In this case, `@validate_call` checks whether `client_name` has at least one character, `client_email` is properly formatted, `items_purchased` is a list of strings, and `amount_owed` is a positive float.

If one of the inputs doesn’t conform to your annotation, Pydantic will throw an error similar to what you’ve seen already with `BaseModel`.

! Pydantic [recommends](https://docs.pydantic.dev/latest/concepts/validation_decorator/#using-field-to-describe-function-arguments) using `Annotated` when you need to validate a function argument that has metadata specified by `Field`.

## Managing Settings

`pydantic-settings` and `BaseSettings`

> If you create a model that inherits from `BaseSettings`, the model initializer will try to read any fields not passed as keyword arguments from environment variables.

```python
class AppConfig(BaseSettings):
    database_host: HttpUrl
    database_user: str = Field(min_length=5)
    database_password: str = Field(min_length=10)
    api_key: str = Field(min_length=20)
```

> `BaseSettings` is not case-sensitive when matching environment variables to field names

! Any validation you can do with `BaseModel` you can also do with `BaseSettings`, including custom validation with model and field validators

### Customizing Settings With SettingsConfigDict

```python
class AppConfig(BaseSettings):
    model_config = SettingsConfigDict(
        env_file=".env",
        env_file_encoding="utf-8",
        case_sensitive=True,
        extra="forbid",
    )

    ...
```

Within your `SettingsConfigDict`, you specify that environment variables should be read from a `.env` file, case-sensitivity should be enforced, and that extra environment variables are forbidden in the `.env` file.

# References:

1. ~~[Pydantic: Simplifying Data Validation in Python](https://realpython.com/python-pydantic/)~~