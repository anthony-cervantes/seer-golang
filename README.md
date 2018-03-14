# Seer Golang Client
[![Build Status](https://travis-ci.org/cshenton/seer-python.svg?branch=master)](https://travis-ci.org/cshenton/seer-python)
[![Python Version](https://img.shields.io/pypi/pyversions/seer.svg)](https://pypi.org/project/seer/)
[![Coverage Status](https://coveralls.io/repos/github/cshenton/seer-python/badge.svg?branch=master)](https://coveralls.io/github/cshenton/seer-python?branch=master)

The golang client for the seer forecasting server.


## Installation

To install the seer client library, use `go get`

```bash
go get github.com/cshenton/seer-golang/seer
```

## Usage

Since this is a client library, you'll need a server to communicate with. To get
up and running just:
```bash
docker run -d -p 8080:8080 cshenton/seer
```
Check out the project repo over [here](https://github.com/cshenton/seer)

Then, to interact over localhost

```go
package main

import (
        "log"
        "time"

       "github.com/cshenton/seer-golang/seer"
)

func main() {
        // Create a client
        c, err := seer.New("localhost:8080")
        if err != nil {
                log.Fatal(err)
        }

        // Create a stream
        _, err = c.CreateStream("myStream", 3600)
        if err != nil {
                log.Fatal(err)
        }

        // Add in data
        _, err = c.UpdateStream(
                "myStream",
                []float64{10, 9, 6},
                []time.Time{
                        time.Date(2016, 1, 1, 0, 0, 0, 0, time.UTC),
                        time.Date(2016, 1, 2, 0, 0, 0, 0, time.UTC),
                        time.Date(2016, 1, 3, 0, 0, 0, 0, time.UTC),
                },
        )
        if err != nil {
                log.Fatal(err)
        }

        // Generate and display forecast
        f, err = c.GetForecast("myStream", 10)
        fmt.Println(f)
}
```