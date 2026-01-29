- {{query (task TODO DOING)}}
-
-
- query-table:: false
  #+BEGIN_QUERY
  {:title "Tasks in Date Range"
  :query [:find (pull ?b [*]) ?person ?project
          :in $ ?start ?end
          :where
          [?b :block/marker ?m] [(contains? #{"TODO" "DOING"} ?m)]
          [?b :block/properties ?props]
          [(get ?props :person) ?person]
          [(get ?props :project) ?project]
          (or-join [?b ?props ?start ?end]
            (and [(get ?props :scheduled) ?d] [(>= ?d ?start)] [(<= ?d ?end)])
            (and [(get ?props :deadline) ?d] [(>= ?d ?start)] [(<= ?d ?end)])
            (and [(get ?props :created) ?d] [(>= ?d ?start)] [(<= ?d ?end)]))]
  :inputs [[:rdt "2026-01-28"] [:rdt "2026-02-28"]]
  :view (table ?b ?person ?project)}
  #+END_QUERY [web:28][web:34]
-