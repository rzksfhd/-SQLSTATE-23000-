// -SQLSTATE-23000-
// SQLSTATE[23000]: Integrity constraint violation: 1062 Duplicate entry ‘indexacion-2-blockTitle’ for key ‘plugin_settings_pkey’ 


// I was trying to upgrade from version 3.2.1-4 to 3.3.0-8 but this error was preventing.
// I found the solution and came to contribute with you, in case you still need it and it is useful for the next ones that come here looking for the solution, as I did.

CREATE TEMPORARY TABLE IF NOT EXISTS plugin_settings_2_delete AS (
    SELECT s.context_id
    FROM plugin_settings s
        LEFT JOIN rt_contexts j ON (s.context_id = j.context_id)
    WHERE j.context_id IS NULL
);
DELETE FROM plugin_settings  WHERE context_id IN (SELECT context_id   FROM plugin_settings_2_delete);
DROP TABLE plugin_settings_2_delete;
