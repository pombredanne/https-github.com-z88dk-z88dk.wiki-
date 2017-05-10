# Time, clock and date functions (time.h)

 | Header     | [{z88dk}/include/time.h](https///raw.githubusercontent.com/z88dk/z88dk/master/include/time.h)    |
 | ---------------------------------------------------------------------------------------------------------------
 | Source     | [{z88dk}/z88dk/libsrc/time](https///github.com/z88dk/z88dk/tree/master/libsrc/time)              |
 | Include    | #include `<time.h>`            |                                                                    
 | Linking    | n/a                          |                                                                    
 | Compile    | n/a                          |                                                                    
 | Comments   |                              |                                                                    

Time functions.  Not all the platforms are supported, and only few functions are supported on platforms other than the Cambridge Z88, for now.


# API

## time_t time(time_t *)

## 'tm' structure

    struct tm {
    int tm_sec;
    int tm_min;
    int tm_hour;
    int tm_mday;
    int tm_mon;
    int tm_year;
    int tm_wday;
    int tm_yday;
    int tm_isdst;
    };


## struct tm *gmtime(time_t *t)

## struct tm *localtime(time_t *t)

## time_t mktime(struct tm *tp)

## clock_t clock(void)


