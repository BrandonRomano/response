<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MPL%202.0-brightgreen.svg" alt="MPL 2.0"></img></a>
  <a href="https://goreportcard.com/report/github.com/BrandonRomano/response">
    <img src="https://goreportcard.com/badge/github.com/BrandonRomano/response" alt="Go Report Card"/>
  </a>
  <a href="https://codecov.io/gh/BrandonRomano/response">
    <img src="https://codecov.io/gh/BrandonRomano/response/branch/master/graph/badge.svg" alt="Code Coverage" />
  </a>
  <a href="https://travis-ci.org/BrandonRomano/response">
    <img src="https://travis-ci.org/BrandonRomano/response.svg?branch=master" alt="Build Status"/>
  </a>
</p>

# Response

Response is a no-frills standardized response body wrapper with some added utility to help manage writing to a [http.ResponseWriter](https://golang.org/pkg/net/http/#ResponseWriter).

## Output Interface

```javascript
{
  "status_code": 200,
  "status_text": "OK",
  "error_details": "Invalid email",
  "result": {
    // ...
  }
}
```

## Usage

```go
func MyHandlerFunc(w http.ResponseWriter, r *http.Request) {
    resp := response.New(w)
    defer resp.Output()

    models, err := getModels()
    if err != nil {
        resp.SetResult(http.StatusInternalServerError, nil).
            WithErrorDetails(err.Error())
        return
    }

    resp.SetResult(http.StatusOK, models)
}
```

## Testing

```go
func TestPingSuccess(t *testing.T) {
    recorder := httptest.NewRecorder()
    req := httptest.NewRequest(http.MethodGet, "/ping", nil)
    PingHandlerFunc(recorder, req)

    assert.Equal(t,
        response.Response{
            StatusCode: 200,
            StatusText: "OK",
            Result:     "pong",
        },
        response.Parse(recorder.Result().Body),
    )
}
```
