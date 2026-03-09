query-table:: false
#+BEGIN_QUERY
{:title [:h2 "⚠️ Overdue"]
  :query [:find (pull ?b [*])
         :in $ ?today
         :where
         [?b :block/marker ?m]
         [(contains? #{"TODO" "DOING" "WAITING"} ?m)]
         (or [?b :block/scheduled ?d] [?b :block/deadline ?d])
         [(< ?d ?today)]]
 :inputs [:today]
 :result-transform (fn [result]
     (sort-by (fn [b]
        [(get b :block/priority "Z") 
         (or (get b :block/scheduled) (get b :block/deadline))]) result))
 :collapsed? false}
 :view (fn [rows] 
        [:div 
         [:div {:class "todo-master-container"} "{{renderer :todomaster}}"])
#+END_QUERY

-
-
- query-table:: false
  #+BEGIN_QUERY
  {:title [:h2 "📋 Actieve Taken"]
   :query [:find (pull ?b [*])
           :in $ ?today
           :where
           [?b :block/marker ?m]
           [(contains? #{"TODO" "DOING"} ?m)]
           (or-join [?b ?today]
             (and (or [?b :block/scheduled ?d] [?b :block/deadline ?d]) [(>= ?d ?today)])
             (and (not [?b :block/scheduled]) (not [?b :block/deadline])))]
   :inputs [:today]
   :result-transform (fn [result]
       (sort-by (fn [b]
          [(get b :block/priority "Z") 
           (or (get b :block/scheduled) (get b :block/deadline) 99999999)]) result))
   :collapsed? false}
  #+END_QUERY
-
- query-table:: false
  #+BEGIN_QUERY
  {:title [:h2 "⏳ Waiting"]
   :query [:find (pull ?b [*])
           :in $ ?today
           :where
           [?b :block/marker "WAITING"]
           (or-join [?b ?today]
             (and (or [?b :block/scheduled ?d] [?b :block/deadline ?d]) [(>= ?d ?today)])
             (and (not [?b :block/scheduled]) (not [?b :block/deadline])))]
   :inputs [:today]
   :result-transform (fn [result]
       (sort-by (fn [b]
          [(get b :block/priority "Z") 
           (or (get b :block/scheduled) (get b :block/deadline) 99999999)]) result))
   :collapsed? false}
  #+END_QUERY
-
-
-