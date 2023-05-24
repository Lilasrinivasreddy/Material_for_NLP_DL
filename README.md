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
