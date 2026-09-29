# mockery-template

[![Donate](https://img.shields.io/badge/%20-Donate-%20?style=flat&logo=githubsponsors&color=E5E4E2)](http://donate.nhat.me)

An extension of the built-in `testify` template. It provides a few more helpers on top:

- `XMocker`: a func type for building a mock with a `*testing.T`.
- `MockX`: creates an `XMocker`, optionally configured with `func(x *X)` setters.
- `NopX`: a ready-to-use mock with no expectations set.

### Usage

```yaml
template: "https://raw.githubusercontent.com/nhatthm/mockery-template/master/templates/testify-mocker.templ"
```

### Example

Given:

```go
type Greeter interface {
	Greet(name string) (string, error)
}
```

The template generates, in addition to the regular `testify` mock:

```go
type GreeterMocker func(tb testing.TB) *Greeter

var NopGreeter = MockGreeter()

func MockGreeter(mocks ...func(greeter *Greeter)) GreeterMocker {
	return func(tb testing.TB) *Greeter {
		tb.Helper()

		greeter := NewGreeter(tb)

		for _, configure := range mocks {
			configure(greeter)
		}

		return greeter
	}
}
```

Usage:

```go
greeter := MockGreeter(func(g *Greeter) {
	g.EXPECT().Greet("world").Return("hi", nil)
})(t)
```

## Donation

If this project saved you some development time, buy me a cup of coffee :)

[![donate](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](http://donate.nhat.me)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;or scan this

<img src="https://github.com/nhatthm/donate.nhat.me/blob/master/images/qr_sponsor.png" width="147px" />
