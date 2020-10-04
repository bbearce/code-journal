# Names and Dates

Who has the same birthday?

This algorithm is a simple, find all people with the same birthday.

```python
# Initial data that we are searching through

data = {'Kate Thompson':'11/23/2000','Ben Patterson':'05/12/1989','Joseph Bernanke':'11/23/1970','Lindsey Harrison':'05/12/2003','Jenny Bennington':'07/09/1998','Jeremy English':'02/27/1967'}

def names_and_dates(data):
    unique_dates = set((i[0:5] for i in data.values()))
    dates_dict = {date:[] for date in unique_dates}
    for date in dates_dict:
        for name in data.keys():
            if data[name][0:5] == date:
                dates_dict[date].append(name)

    same_birthdays = [dates_dict[date] for date in dates_dict.keys() if len(dates_dict[date]) > 1]

    return same_birthdays

same_birthdays = names_and_dates(data)

[print(names, "Have the same birthday") for names in same_birthdays]
```