---
title: 'Quartz: test a cron expression'
author: tonivdv
layout: post
permalink: /2009/08/18/quartz-test-a-cron-expression/
syntaxhighlighter_encoded:
  - 1
dsq_thread_id:
  - 3828085664
categories:
  - General
tags:
  - java
  - quartz
---
<figure style="width: 121px;" class="wp-caption alignright"><img alt="quartz logo" src="http://www.opensymphony.com/quartz/images/quartzEJS.jpg" title="quartz logo" width="121" height="58" /><figcaption class="wp-caption-text">quartz logo</figcaption></figure>Cron expressions in quartz can sometimes be difficult to test, especially when the cron is programmed to trigger after hours or days. Unless you want to wait that time, here is some snippet code you can use to test a cron.

<pre class="brush: java; title: ; notranslate" title="">import java.text.ParseException;
import java.util.Date;

import org.quartz.CronExpression;

public class CronTester {

	public static void main(String[] args) throws ParseException {
		final String expression = "0 0 * * * ?";
        final CronExpression cronExpression = new CronExpression(expression);

        final Date nextValidDate1 = cronExpression.getNextValidTimeAfter(new Date());
        final Date nextValidDate2 = cronExpression.getNextValidTimeAfter(nextValidDate1);

        System.out.println(nextValidDate1);
        System.out.println(nextValidDate2);

	}
}</pre>

Outputs:

<pre class="brush: plain; title: ; notranslate" title="">Tue Aug 18 14:00:00 CEST 2009
Tue Aug 18 15:00:00 CEST 2009</pre>