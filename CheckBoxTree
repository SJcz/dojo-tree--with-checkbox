define(
	['dojo/_base/declare',
	 'dijit/Tree'
	], 
function(declare, Tree){
	var currentNodeIsSelected = false;
	var clazz = declare([Tree], {
		labelShowAs: "id",
		_createTreeNode: function(args){
			var tnode = new dijit._TreeNode(args);
			var arr = this.model.store.query({parent: args.item.id});
			currentNodeIsSelected = false;
			if(args.item.selected){
				currentNodeIsSelected = true;
			}else{
				if(arr.length > 0){
					this.selectOrNot(arr);
				}
			}
     		if(currentNodeIsSelected){
				dojo.place("<input style='vertical-align:middle;' id='" + 
					(args.item,id + "_" + args.item.parent).replace(/\s/g,"_") + 
					"' checked type='checkbox' />", tnode.contentNode, "first");
				tnode.labelNode.innerHTML = args.item[this.labelShowAs];
			}else{
				dojo.place("<input style='vertical-align:middle;' id='" + 
					(args.item.id + "_" + args.item.parent).replace(/\s/g,"_") + 
					"' type='checkbox' />", tnode.contentNode, "first");
				tnode.labelNode.innerHTML = args.item[this.labelShowAs];
			}
			return tnode;
		},
		getIconClass: function(/*dojo.store.Item*/ item, /*Boolean*/ opened){
			return this.model.store.getChildren(item).length ? (opened ? "dijitFolderOpened" : "dijitFolderClosed") : "dijitLeaf";
		},
		onClick: function(item,node,evt){
			var selCb = dojo.byId((item.id + "_" + item.parent).replace(/\s/g,"_"));
			if(selCb == null){
				return;
			}
			if(dojo.hasAttr(selCb, "checked")){
				dojo.destroy(node.contentNode.childNodes[0]);
				dojo.place("<input style='vertical-align:middle;' id='" + 
					(item.id + "_" + item.parent).replace(/\s/g,"_") + 
					"' type='checkbox' />", node.contentNode, "first");
				this.selectAllSub(item.id, item.parent, false);
			}else{
				dojo.destroy(node.contentNode.childNodes[0]);
				dojo.place("<input style='vertical-align:middle;' id='" + 
					(item.id + "_" + item.parent).replace(/\s/g,"_") + 
					"' checked type='checkbox' />", node.contentNode, "first");
				this.selectAllSub(item.id, item.parent, true);
			}
			this.selectAllPar(item.id, item.parent, dojo.byId((item.id + "_" + item.parent).replace(/\s/g,"_")));
		},
		selectAllSub: function(selfId,parentId, isChecked){
			var arr = this.model.store.query({parent: selfId});
			if(!isChecked){
				for(var i=0;i<arr.length;i++){
					dojo.removeAttr(dojo.byId((arr[i].id+"_"+arr[i].parent).replace(/\s/g,"_")), "checked");
					this.selectAllSub(arr[i].id, arr[i].parent, isChecked);
				}
			}else{
				for(var i=0;i<arr.length;i++){
					dojo.attr(dojo.byId((arr[i].id+"_"+arr[i].parent).replace(/\s/g,"_")), "checked", "true");
					this.selectAllSub(arr[i].id, arr[i].parent, isChecked);
				}
			}
		},
		selectAllPar: function(selfId,parentId, selCb){
			var arr = this.model.store.query({id: parentId});
			if(arr.length == 1){
				if(dojo.hasAttr(selCb, "checked")){
					dojo.attr(dojo.byId((arr[0].id+"_"+arr[0].parent).replace(/\s/g,"_")), "checked", "true");
					this.selectAllPar(arr[0].id, arr[0].parent, dojo.byId((arr[0].id+"_"+arr[0].parent).replace(/\s/g,"_")));
				}else{
					var arr2 = this.model.store.query({parent: arr[0].id});
					var isChecked = false;
					for(var i=0;i<arr2.length;i++){
						if(dojo.hasAttr(dojo.byId((arr2[i].id+"_"+arr2[i].parent).replace(/\s/g,"_")), "checked", "true")){
							isChecked = true;
							break;
						}
					}
					if(!isChecked){
						dojo.removeAttr(dojo.byId((arr[0].id+"_"+arr[0].parent).replace(/\s/g,"_")),"checked");
						this.selectAllPar(arr[0].id, arr[0].parent, dojo.byId((arr[0].id+"_"+arr[0].parent).replace(/\s/g,"_")));
					}
				}
			}
		},
		selectOrNot: function(arr){
			  var i;
			  for( i = 0;i<arr.length;i++){
				  if(arr[i].selected){
					  currentNodeIsSelected = true;
					  return;
				  }
			  }
			  if(!currentNodeIsSelected){
				  for( i = 0;i<arr.length;i++){
					  var _arr = this.model.store.query({parent: arr[i].id});
					  if(_arr.length > 0){
						  this.selectOrNot(_arr);
					  }
				  }
			  }
		  }
	});	
	return clazz;		
});
