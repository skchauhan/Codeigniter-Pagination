# Codeigniter-Pagination


		public function index()
		{	
			$config = array();
			$config["base_url"] = base_url() . "index.php/users/index";
			$total_row = $this->user->record_count();
			$config["total_rows"] = $total_row;
			$config["per_page"] = 2;
			$config['use_page_numbers'] = TRUE;
			$config['num_links'] = $total_row;
			$config['cur_tag_open'] = '&nbsp;<a class="current">';
			$config['cur_tag_close'] = '</a>';
			$config['next_link'] = 'Next';
			$config['prev_link'] = 'Previous';

			$this->pagination->initialize($config);
			// echo $this->uri->segment(3);
			if($this->uri->segment(3)){
				$page = ($this->uri->segment(3)) ;
			} else {
				$page = 0;
			}

			$arrUsers['users'] = $this->user->user_list($config["per_page"], $page);
			$str_links = $this->pagination->create_links();
			$arrUsers["links"] = explode('&nbsp;',$str_links );

			$this->load->view('public/user_list', $arrUsers);
		}


		.....................
		Model
		.....................

		public function user_list($limit, $id)
		{
			$this->db->limit($limit, $id);
			$arrUsers = $this->db->get('users');
			echo $this->db->last_query();
			return $arrUsers->result_array();
		}


		public function record_count() {
			return $this->db->count_all("users");
		}

		.....................
		view 
		.....................
	
		<div id="pagination">
			<ul class="tsc_pagination">
				<!-- Show pagination links -->
				<?php foreach ($links as $link) {
				echo "<li>". $link."</li>";
				} ?>
			</ul>
		</div>
