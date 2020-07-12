---
title: "Format a time interval with the requested granularity"
date: "2009-06-25"
tags: [open-source,programming,webdev]
---

This class, a refactored version of Drupal's format\_interval function, makes it relatively easy to format an interval value. The format will automatically format as compactly as possible. For example: if the difference between the two dates is only a few hours and both dates occur on the same day, the year, month, and day parts of the date will be omitted.

```
class DateIntervalFormat
{
    /**
     * Format an interval value with the requested granularity.
     *
     * @param integer $timestamp The length of the interval in seconds.
     * @param integer $granularity How many different units to display in the string.
     * @return string A string representation of the interval.
     */
    public function getInterval($timestamp, $granularity = 2)
    {
        $seconds = time() - $timestamp;
        $units = array(
            '1 year|:count years' => 31536000,
            '1 week|:count weeks' => 604800,
            '1 day|:count days' => 86400,
            '1 hour|:count hours' => 3600,
            '1 min|:count min' => 60,
            '1 sec|:count sec' => 1);
        $output = '';
        foreach ($units as $key => $value) {
            $key = explode('|', $key);
            if ($seconds >= $value) {
                $count = floor($seconds / $value);
                $output .= ($output ? ' ' : '');
                if ($count == 1) {
                    $output .= $key[0];
                } else {
                    $output .= str_replace(':count', $count, $key[1]);
                }
                $seconds %= $value;
                $granularity--;
            }
            if ($granularity == 0) {
                break;
            }
        }

        return $output ? $output : '0 sec';
    }
}
```

Usage:

```
$dateFormat = new DateIntervalFormat();
$timestamp = strtotime('2009-06-21 20:46:11');
print sprintf('Submitted %s ago',  $dateFormat->getInterval($timestamp));
```

Outputs:

```
Submitted 3 days 4 hours ago
```
