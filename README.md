SELECT t1.result_id, t2.status, t1.date
FROM tap_res t1
JOIN Tapsium_AIML.new_results_data t2 ON t1.result_id = t2.result_id
WHERE t2.status IN ('failure', 'success')
AND (t1.date, t2.status) IN (
    SELECT MAX(t1.date), t2.status
    FROM tap_res t1
    JOIN Tapsium_AIML.new_results_data t2 ON t1.result_id = t2.result_id
    WHERE t2.status IN ('failure', 'success')
    GROUP BY t2.status
)
ORDER BY t1.date DESC;


SELECT d.result_id, d.id, d.date1, n.status
FROM Tapsium_AIML.dummy_results_data d
JOIN Tapsium_AIML.new_results_data n ON d.result_id = n.result_id
WHERE n.status IN ('success', 'failure')
AND (d.result_id, d.date1) IN (
  SELECT d2.result_id, MAX(d2.date1)
  FROM Tapsium_AIML.dummy_results_data d2
  JOIN Tapsium_AIML.new_results_data n2 ON d2.result_id = n2.result_id
  WHERE n2.status = n.status
  GROUP BY d2.result_id
)
ORDER BY d.date1 DESC;


