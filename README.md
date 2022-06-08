- 👋 Hi, I’m @faizan913
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
faizan913/faizan913 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
/**
     *  Depreciation calculation
     *  Get asset query for depreciation calculation
     * @return an Array()
     */
    public function getAssetQuery($request)
    {
        $jsonData = array();
        if (($request->category_id == config('depreciation.requested_category')) && ($request->category_id == config('depreciation.requested_category'))) {

            /* $data = Asset::where('status_id', Asset::ASSIGNED_STATUS_ID)
                ->with(
                    ['model' => function ($query) {
                        $query->select('id', 'category_id', 'eol');
                    }]
                )->get(); */
            return DB::table('assets')
                ->select('assets.id', 'assets.name', 'assets.purchase_cost', 'assets.purchase_date', 'models.category_id', 'models.eol', 'assets.model_id')
                ->join('models', 'models.id', '=', 'assets.model_id')
                ->where('assets.status_id', Asset::ASSIGNED_STATUS_ID)
                ->get();
            // $reports =  json_decode($data, true);

        } else if (($request->subCategory == config('depreciation.requested_subcategory'))) {
            return DB::table('assets')
                ->select('assets.id', 'assets.name', 'assets.purchase_cost', 'assets.purchase_date', 'models.category_id', 'models.eol', 'assets.model_id')
                ->join('models', 'models.id', '=', 'assets.model_id')
                ->where('assets.status_id', Asset::ASSIGNED_STATUS_ID)
                ->where('models.category_id', $request->category_id)
                ->get();
            //$jsonData =  json_decode($data, true);
            /* return Asset::where('status_id', Asset::ASSIGNED_STATUS_ID)
                ->with(
                    ['model' => function ($query) use ($request) {
                        $query->select('id', 'category_id', 'eol');
                        $query->where('category_id', $request->category_id);
                    }]
                )->get(); */
            //$jsonData =  json_decode($data, true);
        } else {
            /* $data = Asset::where('status_id', Asset::ASSIGNED_STATUS_ID)
                ->whereIn('model_id', $request->model)
                ->with(
                    ['model' => function ($query) {
                        $query->select('id', 'category_id', 'eol');
                    }]
                )->get(); */
            return  DB::table('assets')
                ->select('assets.id', 'assets.name', 'assets.purchase_cost', 'assets.purchase_date', 'models.category_id', 'models.eol', 'assets.model_id')
                ->join('models', 'models.id', '=', 'assets.model_id')
                ->where('assets.status_id', Asset::ASSIGNED_STATUS_ID)
                ->whereIn('assets.model_id', $request->model)
                ->get();

            // $jsonData =  json_decode($data, true);
        }
        //.return $jsonData;
    }
