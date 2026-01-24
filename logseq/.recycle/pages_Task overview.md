# All Open Tasks Overview
- ## Overdue Tasks (Table)
  
  #+BEGIN_QUERY
  {:view (table ?b ?p ?s)
  :title "🔥 Overdue"
  :query [:find ?b ?p ?s
        :where
        [?b :block/marker "TODO"]
        [?b :block/page ?p]
        [?b :block/scheduled ?s]
        [(missing? $ ?b :block/marker "DONE")]
        [(get-today ?today)]
        [(< ?s ?today)]]}
  #+END_QUERY
  [code_file:1]
- ## Priority Tasks (Table)
  
  #+BEGIN_QUERY
  {:view (table ?b ?p ?priority)
  :title "Priority (A/B/C)"
  :query [:find (pull ?b [:block/content]) ?p ?priority
        :where
        [?b :block/marker ?marker]
        [(contains? #{"TODO" "DOING"} ?marker)]
        [?b :block/page ?p]
        [?b :block/priority ?priority]
        [(not= ?priority "Z")]]
  :result-transform (fn [result]
                    (map (fn [[b p pri]]
                           [(:block/content b) p pri]) result))
  :sort-by [?priority]}
  #+END_QUERY
  [code_file:1]
- ## Open Tickets (Table)
  
  #+BEGIN_QUERY
  {:view (table ?b ?p ?tags)
  :title "Open Tickets (#ticket)"
  :query [:find ?b ?p (pull ?t [:block/name])
        :where
        [?b :block/marker ?marker]
        [(contains? #{"TODO" "DOING"} ?marker)]
        [?b :block/page ?p]
        [?b :block/refs ?t]
        [?t :block/name "ticket"]]
  :result-transform (fn [result]
                    (map (fn [[b p t]]
                           [b p (:block/name t)]) result))}
  #+END_QUERY
  [code_file:1]
-